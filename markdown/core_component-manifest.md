# Creating a component manifest file

Each component type should define a component manifest file that tells the platform what is the name of the component type, how the component is managed and information about the required and optional input attributes for the component type.

The attribute definitions should match to the ones given in the process parameter block for the component in the [Start message](core_msg-start.md).

Each component requires its own manifest file. A template for the component manifest file can be found at [https://github.com/simcesplatform/platform-manager/blob/master/component_manifest.yml](https://github.com/simcesplatform/platform-manager/blob/master/component_manifest.yml).

The component manifest file is done in YAML format which can contain the following attributes:

- `Name` (required)
    - The name for the component type. It must unique, i.e. two different component types cannot have the same name.
- `Type` (required)
    - either "`platform`" for platform managed or "`external`" for externally managed component
- `DockerImage` (required for platform managed component, not needed for externally managed component)
    - the Docker image name including the tag for a platform managed component
- `Description` (optional)
    - description for the component
    - not yet used anywhere in the current version
- `Attributes` (optional)
    - key-value list of the registered attributes for the component
    - each key should be the attribute name that is used in the Start message in the component's process parameter block
    - each value should be an attribute block where each block can have the following attributes:
        - `Optional` (optional) (`true/false`)
            - If `true`, the attribute can be left out when defining a new simulation run.
            - if `false`, a simulation cannot be started without giving a value for this attribute.
            - By default the attribute is set as required, i.e. `Optional` is set to `false`.
        - `Default` (optional)
            - If the default value is set, it is used as the attribute value in cases where the attribute is not given a value in the simulation configuration.
            - This settings is ignored for a required attribute, i.e. if `Optional` is `false`.
        - `Environment` (optional, ignored with externally managed components)
            - If this is given, the corresponding value is used as the environment variable name instead of the attribute name when passing the starting parameters to a platform managed component.
        - `IncludeInStart` (optional) (`true/false`)
            - Whether to include the attribute in the Start message.
            - By default the attribute will be included in the Start message, i.e `IncludeInStart` is set to `true`.
    - Any attribute that is not defined in the component manifest file but is given a value in the simulation configuration file for a new simulation will be included among the parameters in the Start message (and as environment variable for platform managed component).
        - This allows extending the available starting parameters for a component without updating the manifest file but this feature should only be used during the development phase.
        - No checking is done for any of the "undocumented" attributes.

The default filename for the component manifest file is "`component_manifest.yml`" at the root folder of the code repository. It is recommended that this naming is used consistently with all components.
