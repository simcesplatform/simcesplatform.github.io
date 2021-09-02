# Economic Dispatch

## Description

Optimises dispatch of energy purchase and energy storage units given predictions on consumption and production.

## Data

Input for optimisation

| Subscription | Topic | Payload |
| --- | --- | --- |
| ResourceForecast.Load.# and ResourceForecast.Generator.# | ResourceForecastState.(ResourceCategory).(ResourceId) | Forecasts for (non-controllable) resource utilisation |
| PriceForecastState.# | PriceForecastState.(MarketId).(ResourceId) | Electricity price forecasts for each hour |
| ResourceState.Storage.# | ResourceState.Storage.(ResourceId) | Resource initial state (battery state of charge) |
| Request.(MarketId) |  | Flexibility needs from LFM |
| LFMMarketResult.(MarketId) | | Flexibility results from LFM |

Scenario/configuration parameters:

ED and resource parameters given at startup described in Start message blocks

- ED component name from environment variable SIMULATION_COMPONENT_NAME
- Forecast horizon and optimisation time step length as described in Start message blocks (or defaults)
- List of resources to be included as described in Start message blocks
- Resource parameters for optimisation are read from Start message blocs

    - At least storage resource initial kwh, max capacity, etc are read from Start message

- LFM market id (if applicable)

Outside initialisation, the following is needed for flexibility implementation:

- CustomerId relation to resource names (for connection to PGO flexibility Request) is read from Init.CIS.CustomerInfo message

Example configuration file:

```nohighlight
EconomicDispatch: 
      EconomicDispatchA:
            Horizon: PT23H
            Timestep: PT1H
            Resources:
                   - - StaticTimeSeriesResource
                     - Load1
                   - - StaticTimeSeriesResource
                     - Load2
                   - - StaticTimeSeriesResource
                     - Load3
                   - - StaticTimeSeriesResource
                     - Load4
                   - - PriceForecaster
                     - MarketA
                   - - StaticTimeSeriesResource
                     - EV
                   - - StaticTimeSeriesResource
                     - PV_large
                   - - StaticTimeSeriesResource
                     - PV_small
                   - - StorageResource
                     - StorageA
```

## Publish

| Topic | Payload |
| --- | --- |
| ResourceForecastState.Dispatch | The dispatch/schedule |
| Offer.(MarketId) | Flexibility bids to LFM (if applicable) |

## Optimised Resources

The following units are included:

- Static loads and generators

  - Forecasts from Static time series resource, message ResourceForecastState.Power
  - Includes predictions for consumption and production
- Electricity markets

  - Forecasts from PriceForecaster, message PriceForecastState
  - Includes predictions on the price of power to the ED units
- Storages

  - Initial state from Storage resource, message ResourceState
  - Max storage, charge/discharge efficiencies, etc read from start message

Dispatch includes:

- Amount to charge/discharge storage per time interval
- Amount of energy bought from markets per time interval

## Workflow

ED component functionality is message triggered. The optimisation relies on input data and therefore we have a barrier for execution; all input data needs to be received before executing/solving the ED problem:

1. Simulation is started
2. Initialisation as declared above in "Scenario/configuration parameters"
3. Epoch message is received for a new epoch. ED resets barrier arrive counter.
4. ED waits for input data. Arrive counter keeps track of what has arrived. When all input data that is needed has arrived ED solves its optimisation problem. (Input data: Power, Price (, etc) forecasts involved resources.)
5. LFM: If accepted Offers from previous epochs, will receive LFMMarketResult for those
6. ED sends its results in Dispatch message.
7. ED receives state information (StorageResources, state for start of next epoch)
8. LFM: Flexibility Request received → Publish Offer (multiple / if none → publish Offer with zero count)
9. LFM: LFMMarketResult received
10. Status: "Ready"        (If LFM status = "Ready")
11. Repeat steps 3-9.
12. Resource receives a SimState message "stopped" from SimulationManager.
13. Resource closes itself.