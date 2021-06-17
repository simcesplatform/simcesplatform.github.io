# Message structures (core)

This page gathers all message structures of the platform core.


## Type inheritance

To save the re-use of specifications and software, the message structures apply inheritance similar to object-oriented programming. This is illustrated in the following figure:

- AbstractMessage is the base class of all messages
    - All messages MUST inherit fields from this
    - The direct descendants are:
        - SimState
        - AbstractResult
- AbstractResult represents the result (i.e., output) of a process
    - All messages that carry a result MUST inherit the fields of AbstractResult
    - The direct descendants are at least:
        - Epoch
        - Status
        - All actual result messages

![Message hierarchy](images/msg_hierarchy.png)


## Re-usable blocks

Certain structures are generic and re-usable in multiple message structures. These are explained in the following table.

| Structure | Description |
|-|-|
| Quantity array block | Contains an array of quantity values with a unit of measure. |
| Quantity block | Contains a quantity value with a unit of measure. |
| Time series block | Contains one or more time series. |
