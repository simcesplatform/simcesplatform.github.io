## **pppppppppppppppppppppResource Forecaster**

##### Description

ResourceForecaster component predicts / takes a value from historian / etc. for the future status of the resource.

Note! This is static time series version of Resource Forecaster.  

##### Functionalities

1. Predicts the future status of resource

##### ResourceForecaster definition

###### Input parameters

| Property | Datatype | Example |
| --- | --- | --- |
| forecasting\_horizon | ISO 8601 duration | PT36H |

###### Environment variables

| Variable | Example value | Note |
| --- | --- | --- |
| SIMULATION\_COMPONENT_NAME | resource_forecaster | |
| RESOURCE\_TYPES | Load, Generator | List types of forecasted resources. Accepted types are Generator or Load. |
| RESOURCE\_FORECAST\_COMPONENT_IDS | load1, generator1 | This should match with information provided with 'name' input parameter in Resources for each resource to be forecasted |
| RESOURCE\_FORECAST\_STATE\_CSV\_FOLDER | \./ | |
| FORECAST\_HORIZON | PT36H | |

##### Data

Subscribe

| Topic | Payload |
| --- | --- |
|  |  |


Publish

| Topic | Payload |
| --- | --- |
| ResourceForecastState.\(ResourceCategory\).\(ResourceId\) | Real power |

##### Workflow

1. Simulation is started
2. ResourceForecaster receives SimState message "running" from SimulationManager.
3. ResourceForecaster responds to SimulationManager with Status message "ready".
4. ResourceForecaster receives an Epoch message for a new epoch. The epoch number for the first epoch is 1.
5. ResourceForecaster publishes ResourceForecast.Power message to ResourceForecastState topic. Real powers are selected from imported timeseries. Running epoch start time \+ variable 'forecast\_horizon' determines how many real power values are sended. 

    a. ResourceForecaster re-sends ResourceForecast if it receives Epoch message for the running epoch again
	
6. ResourceForecaster sends a Status message with value "ready"
7. Repeat steps 4-6 for each epoch with growing epoch number.
8. ResourceForecaster receives a SimState message "stopped" from SimulationManager.
9. ResourceForecaster closes itself