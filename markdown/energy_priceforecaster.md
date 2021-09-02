# **Price Forecaster**

## Description

PriceForecaster component predicts / takes a value from historian / etc. for the future status of the price. It subscribes to SimState and Epoch topics and publishes to PriceForecastState topic. 

## Functionalities

1. Predicts the future prices

## Input file

The component has pricedata.csv as an input file. The CSV file should contain MarketId, ResourceId, PricingType, UnitOfMeasure, time slots, and prices associated with time slots. Each time slot needs a column and time slots are named as Time1, Time2, Time3, so on. Price associated with each time slot needs a column and time slot prices are named as Price1, Price2, Price3, so on. Needless to mention, the value in PriceX column belongs to the time in TimeX column. Each row contains values which represent data for one epoch. There should be at least as many data rows as the number of simulation epochs. In the CSV file, "." is the decimal separator.

Example content for an input file with 2 epoches and 3 time slots (the first two rows are used for the two epoches):
							Time4	

| MarketId | ResourceId | PricingType | UnitOfMeasure | Time1 | Time2 | Time3 | Price1 | Price2 | Price3 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| MyMarket | NoResource | TOU | {EUR}/(kW.h) | 2020-02-17T10:00:00Z | 2020-02-17T11:00:00Z | 2020-02-17T12:00:00Z | 14.3 | 15.8 | 15.8 |
| MyMarket | NoResource | TOU | {EUR}/(kW.h) | 2020-02-17T11:00:00Z | 2020-02-17T12:00:00Z | 2020-02-17T13:00:00Z | 15.8 | 15.7 | 14.9 |

## Environment variables

| Variable | Example value | Note |
| --- | --- | --- |
| SIMULATION_COMPONENT_NAME | PriceForecaster | |
| PRICE_FORECASTER_STATE_CSV_FILE | pricedata.csv | |

## Workflow of the component

1. Simulation is started
2. PriceForecaster receives SimState message "running" from SimulationManager.
3. PriceForecaster responds to SimulationManager with Status message "ready".
4. PriceForecaster receives an Epoch message for a new epoch. The epoch number for the first epoch is 1.
5. PriceForecaster publishes PriceForecastState
    a. PriceForecaster re-publishes PriceForecastState if it receives Epoch message for the running epoch again
6. PriceForecaster sends a Status message with value "ready"
7. Repeat steps 4-6 for each epoch with growing epoch number.
8. PriceForecaster receives a SimState message "stopped" from SimulationManager.
9. PriceForecaster closes itself.

## Epoch workflow of the component

1. Read next line from CSV file
2. Create a PriceForecastState message based on the data taken from the CSV file
3. Publish the PriceForecastState message
    a. Re-publish PriceForecastState message if Epoch message is received for the running epoch again
4. Publish status ready message

## External packages

The following packages are needed.

| Package | Version | Why needed | URL |
| --- | --- | --- | --- |
| Simulation Tools | | Component implementation based on AbstractSimulationComponent | https://git.ain.rd.tut.fi/procemplus/simulation-tools |
| Domain messages | | Uses the PriceForecastStateMessage class | https://git.ain.rd.tut.fi/procemplus/domain-messages |
