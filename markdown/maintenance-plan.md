# Maintenance plan

This document is a plan to maintain SimCES.
Please note that because SimCES is open source, anyone can exploit it and even publish another version.
Therefore, this document refers to the original SimCES developed by Tampere University and VTT Technical Research Centre of Finland.
Still, PLEASE NOTE that each component of SimCES ships with a license that defines any exploitation-related conditions.


## Terminology

- "Core": any component or aspect of SimCES that is agnostic of any domain-specific simulations
- "RP": responsible person
- "SimCES": the simulation platform "Simulation Environment of Complex Energy System"


## Responsible persons

The responsible persons (RP) are as follows (in alphabetical order):

| Person | Email |
|-|-|
| Antti Keski-Koukkari, VTT | [antti.keski-koukkari@vtt.fi](mailto:antti.keski-koukkari@vtt.fi) |
| Kari Systä, Tampere University | [kari.systa@tuni.fi](mailto:kari.systa@tuni.fi) |
| Petri Kannisto, Tampere University | [petri.kannisto@tuni.fi](mailto:petri.kannisto@tuni.fi) |
| Sami Repo, Tampere University | [sami.repo@tuni.fi](mailto:sami.repo@tuni.fi) |

The group of RPs may change if the existing RPs find this appropriate. 
For instance, this should occur if a person moves to another employer or if SimCES is to be developed further in a new research project.


## Communication and coordination

The primary communication medium between RPs and/or developers is email.

Any error fixes as well as the development of new features are coordinated with this issue board: [https://github.com/orgs/simcesplatform/projects/1](https://github.com/orgs/simcesplatform/projects/1).


## Development of new features

New _core_ features are not expected at the time of writing (Sep 2021) because there are neither resources nor funding.
This means that any new simulation components should comply with what the existing core supports.
