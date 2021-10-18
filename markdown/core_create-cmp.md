# Creating new component

Follow these step to create a new component that participates in simulation in the platform.

1. Make sure you understand the basic concepts of the platform (see [Manual for developer](core_develop-overview.md))
2. <strong>Outputs</strong>: Decide what information the component should generate as result messages
3. <strong>Inputs</strong>: Resolve what information the component needs as the input to generate the result messages
    - This information comes from other components
    - This is optional, because not all components need input
4. <strong>Topics</strong>: Specify the topics the component uses to communicate
    - This is based on "outputs" and "inputs"
    - Re-use existing topics if and only if appropriate
    - Specify new topics as needed
        - See [Conventions of naming](core_conv-name.md)
        - Make sure the names do not conflict with existing topic names (see [Topics (core)](core_topics.md) and [Topics (energy)](energy_topics.md))
5. <strong>Messages</strong>: Determine which data is communicated with "topics"
    - This is based on "outputs" and "inputs"
    - Re-use existing message structures if possible
        - See [Message structures (core)](core_msg.md) and the existing messages at [Message type names (core)](core_msgtype.md) and [Message type names (energy)](energy_msgtype.md)
    - Specify new message structures as needed
        - See [Conventions of messaging](core_conv-msg.md)
6. Choose whether to manage the execution by the platform or externally
    - See [Platform managed and externally managed components](core_cmp-mgmt.md)
7. Develop the component
    - Follow the workflow in pages [Workflow of start and end](core_workflow-start-end.md) and [Workflow of component in simulation](core_workflow-sim.md)
    - When developing a component using Python, it is advisable to to use the [Simulation Tools package](core_sim-tools.md). For details about developing a new component using the package, see the [instructions](https://github.com/simcesplatform/simulation-tools#general-instructions-for-creating-a-new-simulation-component) and the [template file](https://github.com/simcesplatform/simulation-tools/blob/master/examples/component_template.py) for a new component.
    - An example of a platform managed component, called SimpleComponent, has been made by using the [Simulation Tools package](core_sim-tools.md) and the available documentation. The repository, [https://github.com/simcesplatform/simple-component](https://github.com/simcesplatform/simple-component), contains documentation about the development steps taken during the development of the example component.
    - During the component development, things that might have to considered:
        - Environment: Determine what variable information the component must know about the environment (if anything)
        - Parameters: Determine what parametrizable properties the component has (if any)
8. Create a component manifest file using the instructions on page [Creating a component manifest file](core_component-manifest.md).
9. For platform managed component, build and publish a Docker image for the component. See page [Building Docker image for a component](core_docker-image.md) for instructions on how to accomplish that.
10. Update the documentation
    - Document the communication and the workflow of the component. See the page for [Static time series resource](energy_static-time-series-resource.md) for an example.
    - Document any new message types that are used by the component. See the page for [Epoch message](core_msg-epoch.md) for an example.
    - Document the topics used by the component and them to the list topics, for example to [Topics (energy)](energy_topics.md).
    - Document a new Start message block for the component. See [Dummy Component block](core_msg-start.md#dummy-component-block) for an example.
    - For externally managed component, provide full installation instructions for the component.
