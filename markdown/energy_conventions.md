
# Energy-domain-specific conventions

This page documents domain-specific conventions.


## Resource power direction convention

![Resource power direction convention](images/conv_energy_resource_power.svg)

As depicted in figure above the nominal power direction of a single resource is always "Towards the grid". I.e., if the power flows towards the grid, use a positive number. Otherwise, the number must be negative. This convention also applies to power derivative units, like energy.


## Branch current convention

![Branch current convention](images/conv_energy_branch_current.svg)

Network branch element (e.g. lines and transformers) current when considered from either end bus perspective is always "Towards the element".
