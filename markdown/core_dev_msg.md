# Message structures (core)

This page gathers all message structures of the platform core.

Type inheritance
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
