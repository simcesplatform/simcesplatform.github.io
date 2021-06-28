# Logging system

The logging system stores each message in each simulation run to enable viewing the content later.
This enables the analysis of results and debugging of problems.

## Requirements

The identified requirements include:

- Store messages as such (i.e., JSON) along with the necessary metadata
    - Metadata
        - topic
        - timestamp
        - simulation ID
        - warning cause (if any)
        - eno (epoch ID)
        - (message) source
        - others?
- Query for and view messages based on metadata
- Data export
    - In JSON
    - In CSV
        - Data types
            - Time series that contain time series
            - Time series that contain single values
    - Collect a time series from a set of JSON docs
        - E.g., "I want field X.Y.Z from all messages sent to topic A during simulation run B"
        - Should there be a feature to either include timestamps or leave them out?
            - Presumably, the spacing of values in time series is constant anyway, and so is the submission frequency of messages from a particular process
