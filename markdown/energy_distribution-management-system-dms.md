# Distribution management system (DMS)

Distributed management system (DMS) embraces a collection of applications designed for distribution system monitoring and control.
Since DMS performs many tasks, following a distributed approach, various application systems need to operate inside DMS to cover all the tasks as shown in the table below.
Each application system is responsible for one main functionality containing many small functionalities inside.
The service-oriented architecture (SOA) is the idea behind the intended DMS design.
Thanks to SOA, application systems are loosely coupled that enable the distribution of building and developing of each application system to a group of developers.
To achieve modularity and extendability, as mentioned, DMS functionalities are split into distinct application systems, and therefore communications between application systems should be as clear as it is between DMS and outside world.

|DMS application systems   |
| ----------- |
| State monitoring (SM)|
| Predictive grid optimization (PGO):  Network forecast state monitoring and evaluation, utilizing market and non-market based solutions for congestion management, TSO-DSO coordination|
| State estimation (SE)|
| Real-time congestion management (RTCM)|
| Operational grid optimization (OGO)|
| Long-term operational planning|
| Fault management:  Fault location isolation supply restoration (FLISR), outage information to web and customers, reporting to regulator, ...|
| Workforce management|
| Maintenance management|
| Asset management|

## Simulation environment's architecture including DMS

![image](images/dms-structure)
