# AbstractResult

This page specifies the common base of all messages that represent a value calculated, measured or generated in a process. This structure is not meaningful as a standalone message, so it is analogous to an abstract base class in object-oriented programming.


## JSON structure

```nohighlight
(Fields of AbstractMessage must appear too!)

"EpochNumber" : 14,
"IterationStatus" : "final", // leave out when not iterating
"LastUpdatedInEpoch" : 14, // OPTIONAL field
"TriggeringMessageIds" : // TriggeringMessageIds are irrelevant in Epoch messages?
[
  "epoch-14",
  "someprocess-14",
  "anotherprocess-14"
],
"Warnings" : // If warnings precede an error, it can be beneficial to provide them as well in the Error message
[
  "warning.convergence"
],
```


## Fields and multiplicity

| Field | Type | Multiplicity | Explanation |
|-|-|-|-|
| (All fields from [AbstractMessage](core_abstractmessage.md)) |  |  | Fields from the "abstract base class" |
| EpochNumber | UInt | 1 (REQUIRED) | Indicates the running ID of the epoch |
| IterationStatus | String | 0..1; see (1) | Indicates the status of iteration, either "intermediate" or "final" |
| LastUpdatedInEpoch | UInt | 0..1 (OPTIONAL) | Indicates the epoch number when this information was last updated; see (2) |
| TriggeringMessageIds | Array of strings | 1..* (at least one REQUIRED) | The ID of messages that were involved in triggering the calculation of this result |
| Warnings | Array of strings | * (OPTIONAL) | Indicates any warnings that occurred while resolving the Result; see (4) |

- (1): This field is REQUIRED if the component participates in iteration. Otherwise, this field SHOULD be omitted.
- (2): If this field is not present, the recipient SHOULD consider that the information was updated in the current epoch.
- (3)
    - This MUST include the triggering message that was received last
    - This SHOULD include Epoch message if it is among triggering messages
    - As long as the obligatory messages are there, the designer of each process can consider which additional messages to include.
    - This field can be irrelevant in the Epoch message. (Alternatively, Epoch could include the Result received last.)
- (4): See the explanation of causes below. The process SHOULD omit the entire array if no warnings are relevant.


## Warning causes

Each warning MUST specify a cause. The cause MUST either belong to base causes or be a user-defined cause.

### Base causes

The following specifies the base causes. Each base cause has two parts, (1) "warning" followed by period as the separator and (2) a descriptive set of words separated with hyphens.

#### warning.convergence
The calculation failed to converge, or the control has not completely finished. The outcome is either is a default value or calculated with some secondary logic.

#### warning.input
Input is somehow invalid.

#### warning.input-range
One or more inputs are out of range.

#### warning.input-unreliable
Input is unreliable, because the data source (e.g., another process) has indicated a warning.

#### warning.internal
There is something wrong with the internal functionality of the process.

#### warning.other
Other reason; the processes SHOULD NOT use this in the output.



## Open issues

Should there be more information about the warnings, e.g., a more elaborate description for humans to read?
