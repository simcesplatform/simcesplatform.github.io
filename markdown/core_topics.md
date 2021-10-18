# Topics (core)

The following table lists the topic names used by the core platform and which message type occurs in each topic.

**The table below is normative.** This means that if any other page has a conflicting topic name, this table has the precedence.

| Topic name | Message type | Purpose (see 1) | Exchange (see 2) | Publisher(s) | Subscriber(s) | Motivation |
|-|-|-|-|-|-|-|
| Epoch | Epoch | Reporting | S | Simulation Manager | (All non-core components) | Coordinating execution of epochs |
| SimState | SimState | Reporting | S | Simulation Manager | (All non-core components) | Controlling execution of simulation |
| Start | Start | Starting | M | Platform Manager | (All externally managed non-core components) | Signalling start of simulation; deliver parameters |
| Status.Error | Status | Reporting | S | (Any non-core component) | Simulation Manager | Error reporting |
| Status.Ready | Status | Reporting | S | (Any non-core component) | Simulation Manager (+ any non-core component that requires the information about other components readiness) | Reporting readiness to start next epoch |

(1) Purposes:

- Starting: starting a simulation run (once for each simulation run)
- Reporting: communication of data during simulation (multiple times during a simulation run)

(2) Exchanges:

- M: [Management Exchange](core_exchange-mgmt.md)
- S: [Simulation-specific Exchange](core_exchange-sim.md)
