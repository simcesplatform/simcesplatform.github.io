# Topics (energy)

The following table shows which message type occurs in each topic. If a topic has subtopics, there is a dedicated page for it.

**The table below is normative.** This means that if any other page has a conflicting topic name, this table has the precedence.

In many cases, each message structure only occurs in one topic, but a message structure can be reusable between topics as well. For instance, several topics might deliver forecasts using an identical structure. However, in one topic, you likely expect only one message type.

| Topic | Message type | Purpose; see (1) | Publisher(s) | Subscriber(s) |
|-|-|-|-|-|
|  |  | Results |  |  |
|  |  | Results |  |  |
|  |  | Results |  |  |
|  |  | Results |  |  |
|  |  | Results |  |  |
|  |  | Results |  |  |
|  |  | Results |  |  |
|  |  | Results |  |  |
|  |  | Results |  |  |
|  |  | Results |  |  |
|  |  | Results |  |  |
|  |  | Results |  |  |
|  |  | Results |  |  |
|  |  | Results |  |  |
|  |  | Results |  |  |
|  |  | Results |  |  |
|  |  | Results |  |  |
| Offer.(MarketID) | [Offer](energy_msg-offer.md) | Results | [Economic Dispatch](energy_economicdispatch.md) | LFM? |
| Request.(MarketID) | [Request](energy_msg-request.md) | Results | LFM? | [Economic Dispatch](energy_economicdispatch.md) |
| [ResourceForecastState.(ResourceCategory).(ResourceId)](energy_topic-resourceforecaststate.md) | [ResourceForecastState.Power](energy_msg-resourceforecaststate-power.md) | Results | [ResourceForecaster](energy_resourceforecaster.md) | [Economic Dispatch](energy_economicdispatch.md) |
| ResourceForecastState.Dispatch | [ResourceForecastState.Dispatch	](energy_msg-resourceforecaststate-dispatch.md) | Results |  |  |
| [ResourceForecastState.(ResourceCategory).(ResourceId)](energy_topic-resourceforecaststate.md) | [ResourceState](energy_msg-resourcestate.md) | Results | (Resources) | [Grid (DSS)](energy_grid-dss.md) |
| SelectedOffer.(MarketID) | [SelectedOffer](energy_msg-selectedoffer.md) | Results |  |  |

TODO: add table content


(1) Purposes:

- Initialization: initializing before the actual simulation cycles have begun (once for each simulation run)
- Results: communication of result data during simulation (multiple times during a simulation run)
