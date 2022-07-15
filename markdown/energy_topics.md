# Topics (energy)

The following table shows which message type occurs in each topic. Some topics have even a dedicated page for additional information.

**The table below is the primary source of topic-related information.** If any other page conflicting topic information, this table has the precedence.

In many cases, each message structure only occurs in one topic, but a message structure can be reusable between topics as well. For instance, several topics might deliver forecasts using an identical structure. However, in one topic, you likely expect only one message type.

| Topic (1) | Message type | Publisher(s); see (2) | Subscriber(s); see (2) |
|-|-|-|-|
| ControlState.PowerSetpoint | [ControlState.PowerSetpoint](energy_msg-controlstate-powersetpoint.md)| [Controller](energy_controller.md) | [Storage](energy_storage.md) |
| DMSNetworkState.Voltage | [DMSNetworkState.Voltage](energy_msg-dmsnetworkstatus-voltage.md) | [Distribution management system (DMS)](energy_distribution-management-system-dms.md) - SM | [Distribution management system (DMS)](energy_distribution-management-system-dms.md) - RTCM |
| DMSNetworkState.Current | [DMSNetworkState.Current](energy_msg-dmsnetworkstatus-current.md) | [Distribution management system (DMS)](energy_distribution-management-system-dms.md) - SM | [Distribution management system (DMS)](energy_distribution-management-system-dms.md) - RTCM |
| FlexibilityNeed.(MarketID) | [FlexibilityNeed](energy_msg-flexibilityneed.md) | [Distribution management system (DMS)](energy_distribution-management-system-dms.md) - PGO | [Procem-LFM](energy_procem-lfm.md) |
| Init.CIS.CustomerInfo | [Init.CIS.CustomerInfo](energy_msg-init-cis-customerinfo.md) | [Grid (DSS)](energy_grid-dss.md) | [Distribution management system (DMS)](energy_distribution-management-system-dms.md) - PGO, SM |
| Init.NIS.NetworkBusInfo | [Init.NIS.NetworkBusInfo](energy_msg-init-nis-networkbusinfo.md) | [Grid (DSS)](energy_grid-dss.md) | [Distribution management system (DMS)](energy_distribution-management-system-dms.md) - PGO, SM |
| Init.NIS.NetworkComponentInfo | [Init.NIS.NetworkComponentInfo](energy_msg-init-nis-networkcomponentinfo.md) | [Grid (DSS)](energy_grid-dss.md) | [Distribution management system (DMS)](energy_distribution-management-system-dms.md) - PGO, SM |
| LFMMarketResult.(MarketId) | [LFMMarketResult](energy_msg-lfmmarketresult.md) | [Procem-LFM](energy_procem-lfm.md) | [Economic Dispatch](energy_economicdispatch.md) |
| LFMOffering.(PGOId) | [LFMOffering](energy_msg-lfmoffering.md) | [Procem-LFM](energy_procem-lfm.md) | [Distribution management system (DMS)](energy_distribution-management-system-dms.md) - PGO |
| NetworkForecastState.(GridId).Voltage.(BusName) | [NetworkForecastState.Voltage](energy_msg-networkforecaststate-voltage.md) | [Grid (DSS)](energy_grid-dss.md) | [Distribution management system (DMS)](energy_distribution-management-system-dms.md) - PGO |
| NetworkForecastState.(GridId).Current.(DeviceId) | [NetworkForecastState.Current](energy_msg-networkforecaststate-current.md) | [Grid (DSS)](energy_grid-dss.md) | [Distribution management system (DMS)](energy_distribution-management-system-dms.md) - PGO |
| NetworkState.(GridId).Current.(DeviceId) | [NetworkState.Current](energy_msg-networkstate-current.md) | [Grid (DSS)](energy_grid-dss.md) | [Distribution management system (DMS)](energy_distribution-management-system-dms.md) - SM |
| NetworkState.(GridId).Voltage.(BusName) | [NetworkState.Voltage](energy_msg-networkstate-voltage.md) | [Grid (DSS)](energy_grid-dss.md) | [Distribution management system (DMS)](energy_distribution-management-system-dms.md) - SM |
| NetworkState.(GridId).Loss.(DeviceId) | [NetworkState.Loss](energy_msg-networkstate-loss.md) | [Grid (DSS)](energy_grid-dss.md) | [Distribution management system (DMS)](energy_distribution-management-system-dms.md) - SM |
| Offer.(MarketID) | [Offer](energy_msg-offer.md) | [Economic Dispatch](energy_economicdispatch.md) | [Procem-LFM](energy_procem-lfm.md) |
| Request.(MarketID) | [Request](energy_msg-request.md) | [Procem-LFM](energy_procem-lfm.md) | [Economic Dispatch](energy_economicdispatch.md) |
| [ResourceForecastState.(ResourceCategory).(ResourceId)](energy_topic-resourceforecaststate.md) | [ResourceForecastState.Power](energy_msg-resourceforecaststate-power.md) | [ResourceForecaster](energy_resourceforecaster.md) | [Economic Dispatch](energy_economicdispatch.md) |
| ResourceForecastState.Dispatch | [ResourceForecastState.Dispatch	](energy_msg-resourceforecaststate-dispatch.md) | [Economic Dispatch](energy_economicdispatch.md) | [Controller](energy_controller.md) |
| [ResourceState.(ResourceCategory).(ResourceId)](energy_topic-resourcestate.md) | [ResourceState](energy_msg-resourcestate.md) | (Resources) | [Grid (DSS)](energy_grid-dss.md) |
| SelectedOffer.(MarketID) | [SelectedOffer](energy_msg-selectedoffer.md) | [Distribution management system (DMS)](energy_distribution-management-system-dms.md) - PGO | [Procem-LFM](energy_procem-lfm.md) |

(1) Topic name:

- If the name starts with "Init", the purpose of the topic is initialization _before_ the actual simulation cycles begin, once for each simulation run
- Otherwise, the purpose is the communication of result data (multiple times during a simulation run)

(2) Abbreviations:

- PGO: Predictive Grid Optimization
- RTCM: Real-time Congestion Management
- SM: State Monitoring
