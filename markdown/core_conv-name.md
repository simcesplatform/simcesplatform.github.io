# Conventions of naming

It helps development if the various communication-related items have been named consistently. Then, the developers encounter fewer surprises, which often lead to bugs.

The following sections specify the naming conventions of various items.

## Cases

These are the cases referred to on this page:

- (a) Kebab case
    - E.g., "kebab-case"
    - All letters are in lowercase
    - Hyphen between each word
    - No spaces or underscores
- (b) Pascal case: camel case with capital first letter
    - E.g. "PascalCase"
    - Each word starts with a capital letter
    - Other letters are in lowercase
    - No spaces, underscores or hyphens between words
    - See [https://techterms.com/definition/pascalcase](https://techterms.com/definition/pascalcase)


## All items

This includes message names, field names, etc.

- You SHOULD avoid the use of abbreviations

For example, say "Voltage" rather than "V".


## Exchanges (in AMQP)

- Case: Kebab case
- If the name is hierarchical or otherwise contains logical parts, you CAN use periods ('.') as the separator.
- You SHOULD only use alphanumeric characters and the separators (hyphen and period, i.e., '-' and '.').

Examples:

- procem-management
- procem.whatever-id-1234


## Topics (in AMQP)

- Case: Pascal case
- You SHOULD use periods to separate the levels of a hierarchy or other logical sections, because this enables wildcards to match a section.
- All words SHOULD be in singular.
- Any topic that has the purpose "initialization" SHOULD start with string "Init.". Any other topic SHOULD NOT start with "Init.". (For more information about initialization, see [Workflow of component in simulation](core_workflow-sim.md).)
- Any topic meant for the intermediate results of iteration MUST end with ".Iter" added to the topic name that delivers the final result of iteration. Any other topic SHOULD NOT end with ".Iter". (For more information about iteration, see [Workflow of component in simulation](core_workflow-sim.md).)

Examples:

- ResourceState.Generator.Generator1
- Init.NIS.NetworkComponentInfo
- MyTopic.ResourceX
- MyTopic.ResourceX.Iter


## Queues (in AMQP)

- You SHOULD use randomly generated names to avoid naming conflicts between software applications. When the queues are exclusive to applications, there is no reason to use an explicit name. Commonly, the APIs generate a random name if no name is supplied.


## Field names in message

- Case: Pascal case
- You SHOULD use singular if there is one item in the field.
- If the field contains an array, you SHOULD use plural.

Examples:

- LastUpdatedInEpoch
- SourceProcessId


## Message type

- Case: Pascal case
- All words SHOULD be in singular.

Examples:

- PriceForecastState
- ResourceState


## Enumeration values

- Case: Kebab case
- If hierarchical, you SHOULD use periods ('.') as the separator.
- You SHOULD only use alphanumeric characters and the separators (hyphen and period, i.e., '-' and '.'). Still, numbers SHOULD be used only if absolutely necessary.

Examples:

- warning.convergence
- warning.input.range
- ready
- my-multiword-enum-value
- my-multiword-enum-value.sub-item
    