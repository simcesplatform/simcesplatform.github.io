# Start message blocks

This page contains the domain-specific parameter blocks included in the start message (see [Start (message)](core_msg-start.md)).

## JSON structure

```nohighlight
{

  "ProcessParameters" :
  {
    "EconomicDispatch": // Include this if relevant
    {
      "EconomicDispatchA": // Use a name because there can be multiple economic dispatches
      {
        "Horizon": "PT36H",  // optional 
        "Timestep": "PT1H",  // optional
        "Resources": [
          ["StaticTimeSeriesResource", "LoadA"],
          ["StaticTimeSeriesResource", "GeneratorA"],
          ["StorageResource", "storageA"]
        ],
        "Weights": {   // optional  -- See below for examples
          ...
        },
        "ParticipatingMarketId": [ "MarketA" ],    // LFM Market Id if applicable
        "CommitmentTime": "12:00",
        "SkipOpenOffers": "True"
      },
      "EconomicDispatchB":
      {
        "Horizon": "PT36H",  // optional 
        "Timestep": "PT1H",  // optional 
        "Resources": [
          ["StaticTimeSeriesResource", "LoadB"],
          ["StaticTimeSeriesResource", "GeneratorB"],
          ["StorageResource", "storageB"],
          ["PriceForecaster", "marketB"]
        ],
        "ParticipatingMarketId": [ "MarketA" ]  // optional
      }
    },

    "Grid": // Include this if relevant
    {
      "GridA": // Use a name because there can be multiple grids
      {
        "ModelName" : "set_model_name_or_identifier_here",
        "Resources": [
           ["LoadA","bus3"],
           ["GeneratorA","bus5"]
        ],
        "Symmetrical" : false, //can be omitted, default is false
        "MaxControlCount" : 15 //can be omitted, default is 15
      },
      "GridB":
      {
        "ModelName" : "set_model_name_or_identifier_here",
        "Resources" : [],
        "Symmetrical" : false, //can be omitted, default is false
        "MaxControlCount" : 15 //can be omitted, default is 15
      }
    },

    "PredictiveGridOptimization": // Include this if relevant
    {
      "PredictiveGridOptimizationA": // Use a name because there can be multiple grids
      {
        "MonitoredGridName" : "GridA",
        "FlexibilityNeedMargin" : 0.2 ,
        "MaxVoltage" : 1.05,
        "MinVoltage" : 0.95,
        "UpperAmberBandVoltage" : 0.01,
        "LowerAmberBandVoltage" : 0.01,
        "OverloadingBaseline" : 1,
        "AmberLoadingBaseline" : 0.9
      },
      "PredictiveGridOptimizationB":
      {
        "MonitoredGridName" : "GridB",
        "RelativeSensitivity" : 45 ,
        "FlexibilityNeedMargin" : 0.2 ,
        "MaxVoltage" : 1.05,
        "MinVoltage" : 0.95,
        "UpperAmberBandVoltage" : 0.01,
        "LowerAmberBandVoltage" : 0.01,
        "OverloadingBaseline" : 1,
        "AmberLoadingBaseline" : 0.9
      }
    },

    "StateMonitoring": // Include this if relevant
    {
      "SM_A": // Use a name because there can be multiple grids
      {
        "MonitoredGridName" : "GridA",
        "MaxVoltage" : 1.05,
        "MinVoltage" : 0.95,
        "UpperAmberBandVoltage" : 0.01,
        "LowerAmberBandVoltage" : 0.01,
        "OverloadingBaseline" : 1,
        "AmberLoadingBaseline" : 0.9
      },
      "SM_B":
      {
        "MonitoredGridName" : "GridB",
        "MaxVoltage" : 1.05,
        "MinVoltage" : 0.95,
        "UpperAmberBandVoltage" : 0.01,
        "LowerAmberBandVoltage" : 0.01,
        "OverloadingBaseline" : 1,
        "AmberLoadingBaseline" : 0.9
      }
    },

    "StaticTimeSeriesResourceForecaster": // Include this if it is relevant
    {
      "ResourceForecaster": // The identifier for the resource forecaster
      {
        "ResourceForecastComponentIds": "load1, generator1",
        "ResourceTypes": "Load, Generator",
        "ResourceForecastStateCsvFolder": "/resources/forecasts/"
      }
    },

    "StaticTimeSeriesResource":  // Include this if it is relevant
    {
      "LoadA":  // The identifier for the resource
      {
        "ResourceType": "Load",
        "ResourceStateFile": "load.csv",
        "ResourceFileDelimiter": ","  // optional, default value is ","
      },
      "GeneratorA":
      {
        "ResourceType": "Generator",
        "ResourceStateFile": "generator.csv"
      }
    },

    "PriceForecaster": // Include this if it is relevant
    {
      "PriceForecasterA": // Use a name because there can be multiple price forecasters
      {
        "PriceForecasterStateCsvFile": "priceA.csv",
        "PriceForecasterStateCsvDelimiter": "," // optional, default value is ","
      },
      "PriceForecasterB": 
      {
        "PriceForecasterStateCsvFile": "priceB.csv"
      }
    },

    "StorageResource":  // Include this if it is relevant
    {
      "storageA":  // The identifier for the storage
      {
        "ResourceStateCsvFile": "control.csv",
        "ResourceStateCsvDelimiter": ",",
        "Bus": "bus1",
        "ChargeRate": 100.0,
        "DischargeRate": 100.0,
        "InitialStateOfCharge": 50.0, 
        "KwhRated": 100.0,
        "DischargeEfficiency": 90.0,
        "ChargeEfficiency": 90.0,
        "KwRated": 100.0,
        "SelfDischarge": 0.0
      }
    }
      
    "ProcemLFM":
    {
      "LFM1":
      {
        "MarketOpeningTime": 14,
        "MarketClosingTime": 17,
        "FlexibilityProviderList": ["economic-dispatch1","economic-dispatch1"],
        "FlexibilityProcurerList": ["pgo1"]
      }
    }
  }
}
```

## Process parameter blocks

### Economic dispatch block

Note: the Platform Manager passes these parameters directly to the Economic Dispatch instance through environmental variables. They are included as part of the Start message to make the used parameters visible to the other components as well as to the Logging System.

| Field | Type | Multiplicity | Explanation |
| --- | --- | --- | --- |
| Horizon | ISO 8601 duration; e.g. 36 hours: "PT36H" | 0..1 (OPTIONAL) | Optimisation problem horizon (length of time range). If omitted, the default (36 h) is applied. NOTE: This must be compatible with the received forecasts, i.e. regarding forecast length |
| Timestep | ISO 8601 duration; e.g. 1 hour interval: "PT1H" (DEFAULT) or 15 min: "PT15M" | 0..1 (OPTIONAL) | The length of timestep. If omitted, the default (1 h) is applied. Note: This must be compatible with the received forecasts, i.e. regarding forecast resolution |
| Resources | N by 2 array of string | 1 (REQUIRED) | Array of component keys and resources in the economic dispatch scenario. Component key identifies component type. Allowed keys (currently): "StaticTimeSeriesResource", "PriceForecaster", "StorageResource" |
| Weights | Dictionary with identifier for (storage) resource (or "default" if to be applied to all). Fields (1) TerminalSOCBound: Percentage (0-100), Default: 40.0. (2) TerminalSOCTarget: Percentage (0-100), Default: None. (3) TerminalWeight: Unitless, in effect EUR/kwh, Default: None | 0..1 (OPTIONAL) | Optimisation weights, See (1) below table |
| ParticipatingMarketId | String | 0..1 (OPTIONAL), REQUIRED if LFM is to be used | MarketId for LFM. Required for participation in LFM. (Name is not checked - will operate if none is given. ED epoch "Ready" status is dependent on LFM "Ready" status, so will never be ready if wrong name is given.) |
| CommitmentTime | String | 0..1 (OPTIONAL) | Time at which day-ahead market closes (for next day). Given as a string with hours and minutes (must coincide with start time of epoch). For example "12:00". Commits to next day electricity market values at 12:00. Next day: midnight to midnight. |
| SkipOpenOffers | String | 0..1 (OPTIONAL) | Ignores open offers when calculating LFM offers. String valued. Accepted values: "True"/"False". |

(1) Optimisation weights:
- Case 1: TerminalSOCBound given and TerminalSOCTarget + TerminalWeight not given.
  - Limits the storage state on the last time instant in optimisation horizon with the lower bound TerminalSOCBound

For example:
```nohighlight
"Weights":  
  {
    "storageA":  // Identifier for resource component  
    {
      "TerminalSOCBound": 50.0
    }
  }
```

- Case 2: TerminalSOCBound, TerminalSOCTarget, TerminalWeight given
  - Limits the storage state on the last time instant with the lower bound of TerminalSOCTarget - TerminalSOCBound
  - Limits the storage state on the last time instant with the upper bound of TerminalSOCTarget + TerminalSOCBound (i.e. maximum deviation)
  - Deviation from TerminalSOCTarget on last time instant minimized in either direction by linearly weighting with TerminalWeight

For example:
```nohighlight
"Weights": 
  {
    "storageB":  // Identifier for resource component  
    {
      "TerminalSOCBound": 10.0,
      "TerminalSOCTarget": 40.0,
      "TerminalWeight": 30.0
    }
  }
```

### Grid block

| Field | Type | Multiplicity | Explanation |
| --- | --- | --- | --- |
| ModelName	| String | 1 (REQUIRED) | String referring to a network model in Grid model library. If erroneous or missing will cause an error in Grid process. Case-insensitive. |
| Resources | N by 2 array of string | 0..1 (OPTIONALish) | Array of resources and buses to which they are connected that the grid instance should consider. If omitted grid collects all resource names from any parameter block thats name ends in "Resource" and uses their default locations in the grid model. If no default location exists the resource is not considered. NOTE: It's very dangerous to omit this field!!! |
| Symmetrical | Boolean | 0..1 (OPTIONAL) | Indicator whether or not the grid should be considered symmetrical. If omitted assumed FALSE. | 
| MaxControlCount | Integer ( > 0 ) | 0..1 (OPTIONAL) | Maximum number of control iteration allowed for Grid's internally modelled controllers per loadflow. Default if omitted is 15. Purpose of this variable is to interrupt set point hunting and ensure convergence. NOTE: This variable only impacts the Grid modelled controllers, external controllers have to keep their own iteration count. |
| ModelVoltageBand | Float ( < 1, > 0 ) | 0..1 (OPTIONAL) | The +/- band of voltages where nominal voltage dependency of resources is assumed to apply e.g. ModelVoltageBand=0.1 would assume that the dependency models apply between 0.9 to 1.1 pu. Beyond this band the model is assumed to revert to constant impedance to ensure convergence. If omitted 0.15 is used. |
| ForecastFlows | Boolean | 0..1 (OPTIONAL) | Indicator whether or not the Grid should also calculate forecasted load flows. Can be omitted, default is false. Set true if PGO is part of the simulation | 

### PredictiveGridOptimization block

| Field | Type | Multiplicity | Explanation |
| --- | --- | --- | --- |
| MonitoredGridName | String | 1 (REQUIRED) | String referring to a Grid instance in the running simulation (e.g. "GridA") |
| RelativeSensitivity |	Integer	| 1 (REQUIRED) | RelativeRessitivity (RS) defines the size of a congestion area. 100 means the largest possible. 0 means the smallest possible. 0<=RS=<100 |
| FlexibilityNeedMargin | Float (  > -1, < 1 ) | 1 (REQUIRED) | FlexibilityNeedMargin (FNM) defines adjustment of the flexibility need's volume: if positive, over-purchase  if negative, under-purchase  if zero, neutral|
| MaxVoltage | Float (  > 0, > MinVoltage ) | 1 (REQUIRED) | Maximum allowable voltage for customer connected nodes (p.u.). Violation of this voltage causes RED condition. (identical for both MV and LV lines) |
| MinVoltage | Float (  > 0, < MaxVoltage) | 1 (REQUIRED) |	Minimum allowable voltage for customer connected nodes (p.u.). Violation of this voltage causes RED condition. (identical for both MV and LV lines) |
| UpperAmberBandVoltage | Float (MaxVoltage - UpperAmberBand > MinVoltage + LowerAmberBand, >0) | 1 (REQUIRED) | Voltage margin for AMBER condition of overvoltage. (p.u.) |
| LowerAmberBandVoltage | Float (MaxVoltage - UpperAmberBand > MinVoltage + LowerAmberBand, >0) | 1 (REQUIRED) | Voltage margin for AMBER condition of undervoltage. (p.u.) |
| OverloadingBaseline | Float ( >0 ) | 1 (REQUIRED) | Component loading limit that is considered RED condition (typically 1, corresponding to 100% loading). |
| AmberloadingBaseline | Float ( >0, <= OverloadingBaseline) | 1 (REQUIRED) | Component loading limit that is considered AMBER condition (e.g., 0.9) |

### StateMonitoring block

| Field | Type | Multiplicity | Explanation |
| --- | --- | --- | --- |
| MonitoredGridName	| String | 1 (REQUIRED) | String referring to a Grid instance in the running simulation (e.g. "GridA") |
| MaxVoltage | Float (  > 0, > MinVoltage ) | 1 (REQUIRED) | Maximum allowable voltage for customer connected nodes (p.u.). Violation of this voltage causes RED condition. (identical for both MV and LV lines) |
| MinVoltage | Float (  > 0, < MaxVoltage) | 1 (REQUIRED) | Minimum allowable voltage for customer connected nodes (p.u.). Violation of this voltage causes RED condition. (identical for both MV and LV lines) |
| UpperAmberBandVoltage | Float (MaxVoltage - UpperAmberBand > MinVoltage + LowerAmberBand, >0) | 1 (REQUIRED) | Voltage margin for AMBER condition of overvoltage. (p.u.) |
| LowerAmberBandVoltage | Float (MaxVoltage - UpperAmberBand > MinVoltage + LowerAmberBand, >0) | 1 (REQUIRED) | Voltage margin for AMBER condition of undervoltage. (p.u.) |
| OverloadingBaseline |	Float ( >0 ) | 1 (REQUIRED) | Component loading limit that is considered RED condition (typically 1, corresponding to 100% loading) |
| AmberloadingBaseline | Float ( >0, <= OverloadingBaseline) | 1 (REQUIRED) | Component loading limit that is considered AMBER condition (e.g., 0.9) |

### Static time series block

Note: the Platform Manager passes these parameters directly to the Static Time Series Resource instance through environmental variables. They are included as part of the Start message to make the used parameters visible to the other components as well as to the Logging System.

| Field | Type | Multiplicity | Explanation |
| --- | --- | --- | --- |
| ResourceType | String | 1(REQUIRED) | The type of the resource. MUST be either "Load" or "Resource". |
| ResourceStateFile | String | 1 (REQUIRED) | The name of the CSV file containing the static time series. |
| ResourceFileDelimiter | String | 0..1 (OPTIONAL) | The delimiter that is used in the CSV file. The default delimiter is comma, i.e. ",". |

### Storage resource block

Note: the Platform Manager passes these parameters directly to the Storage Resource instance through environmental variables. They are included as part of the Start message to make the used parameters visible to the other components as well as to the Logging System.

| Field | Type | Multiplicity | Explanation |
| --- | --- | --- | --- |
| ResourceStateFile | String | 0..1 (OPTIONAL) | The name of the CSV file containing control state information. |
| ResourceStateDelimiter | String | 0..1 (OPTIONAL) | The delimiter that is used in the CSV file. The default delimiter is comma, i.e. ",". |
| Bus | String | 0..1 (OPTIONAL) | Name of bus to which the resource is connected. Required if ResourceStateCsvFile is not given. |
| Node | String | 0..1 (OPTIONAL) | Node that 1-phase resource is connected to. Possible values 1, 2 and 3. Used if file state source is not used and if then this is not specified then it is assumed that the resource is 3-phase resource. |
| ChargeRate | Float | 0..1 (OPTIONAL) | Charging rate (input power) in Percent of rated kW. Default value 100.0. |
| DischargeRate | Float | 0..1 (OPTIONAL) | Discharge rate (output power) in Percent of rated kW. Default value 100.0. |
| InitialStateOfCharge | Float | 1 (REQUIRED) | Initial amount of energy stored, %. |
| KwhRated | Float | 1 (REQUIRED) | Rated storage capacity in kWh. |
| DischargeEfficiency | Float | 0..1 (OPTIONAL) | Percent, efficiency for discharging the storage. Default value 90.0. |
| ChargeEfficiency | Float | 0..1 (OPTIONAL) | Percent, efficiency for charging the storage. Default value 90.0. |
| KwRated | Float | 1 (REQUIRED) | kW rating of power output. | 
| SelfDischarge | Float | 0..1 (OPTIONAL) | Percent of rated kWh drained from storage while idling. Default value 0.0. |

### Static Time Series Resource Forecaster block

Note: the Platform Manager passes these parameters directly to the Static Time Series Resource Forecaster instance through environmental variables. They are included as part of the Start message to make the used parameters visible to the other components as well as to the Logging System.

| Field | Type | Multiplicity | Explanation |
| --- | --- | --- | --- |
| ResourceForecastComponentIds | String | 1 (REQUIRED) | List names (strings) of forecasted resources. This information should match with csv file names that contain resource forecast state information. |
| ResourceTypes | String | 1 (REQUIRED) | List types (strings) of forecasted resources. Accepted values are Generator or Load. |
| ResourceForecastStateCsvFolder | String | 1 (REQUIRED) | Location of the folder that contains csv files. Csv files contain the resource forecast state information used in the simulation. Relative file paths are in relation to the current working directory. |
| ResourceStateCsvDelimiter | String | 0..1 (OPTIONAL) | The delimiter that is used in the CSV file. The default delimiter is comma, i.e. ",". |
| ResourceType | String | 0..1 (OPTIONAL) | Type of this resource. Default is "ResourceForecaster". |
| ResourceForecastTopic | String | 0..1 (OPTIONAL) | The upper level topic under whose subtopics the resource states are published. If this environment variable is not present "ResourceForecastState" is used. |
| ForecastHorizon | String | 0..1 (OPTIONAL) | If this environment variable is not present then default "PT36H" is used. |
| UnitOfMeasure | String | 0..1 (OPTIONAL) | If this environment variable is not present then default "kW" is used. |

### LFM block

| Field | Type | Multiplicity | Explanation |
| --- | --- | --- | --- |
| MarketOpeningTime | Integer [0 ... 24] | 1 (REQUIRED) | Hour of day when market opens |
| MarketClosingTime | Integer [0 ... 24] | 1 (REQUIRED) | Hour of day when market closes |
| FlexibilityProviderList | Array of String | 1 (REQUIRED) | List of SourceProcessIds for processes that are expected to send FlexibilityNeed msgs |
| FlexibilityProcurerList | Array of String | 1 (REQUIRED) | List of SourceProcessIds for processes that are expected to send Offer msgs |
