# AbstractSimulationComponent

`AbstractSimulationComponent` is a base class that contains the common functionality of simulation components. It is written in Python (3.7.9).

The source code for the class can be found at: [https://github.com/simcesplatform/simulation-tools/blob/master/tools/components.py](https://github.com/simcesplatform/simulation-tools/blob/master/tools/components.py)

New components can be developed by creating a child class from the `AbstractSimulationComponent` and providing an implementation for certain methods.

A template for creating a child class, [https://github.com/simcesplatform/simulation-tools/blob/master/examples/component_template.py](https://github.com/simcesplatform/simulation-tools/blob/master/examples/component_template.py), is available and other information regarding the creation of a new simulation component can be found at [Creating new component](core_create-cmp.md).

## The workflow during the initialization process

A diagram describing the the workflow at the initialization process for a new component based on AbstractSimulationComponent using the template for creating a child class. In the the diagram the class name for the new component is `NewComponent`.

![Workflow diagram at the initialization process for a new component based on AbstractSimulationComponent using the template for creating a child class.](images/abstract_component_class_initialization.png)

The parts that the developer are required to either implement or modify are marked with red color in the diagram. These include:

- `create_component`
    - this method should return a new instance of the new component
    - The recommended workflow:
        - read in the environmental variables required for the new component ("Read NewComponent specific environment variables" in the diagram)
        - create an instance of the new component by calling its constructor with the parameters
        - return the created instance.
- constructor of the new component
    - Initialize `NewComponent` specific variables
        - This part of the constructor should initialize any variables inside the new component that are required to keep track of all the required information that the component needs to know when the simulation is running.
    - Set up other topics variable
        - This part of the constructor should initialize the `self._other_topics` variable with a list of the topics that the component needs to listen to while the simulation is running.
    - Set up initialization error if all variables could not be properly initialized
        - If there was some error when initializing one or more of the component variables, `self.initialization_error` should be set up with an appropriate error message.

## The workflow when receiving a new message

A diagram describing the workflow inside the component after receiving a new message from the message bus is given below. Each box represents a method in the class.

![Full workflow diagram for AbstractSimulationComponent when receiving a new message from the message bus](images/abstract_component_workflow.png)

Those methods that require an implementation in the child class are marked with red color in the diagram. These include:

- `clear_epoch_variables`
    - this method should reset all the variables that are used to store epoch specific information
- `general_message_handler`
    - any received message other than Epoch or SimState message should be handled here
    - a call to `start_epoch` is required when a new valid input message has been received
- `ready_for_new_epoch`
    - should return `true` when enough input messages have been received to be able to do something in the current epoch
- `process_epoch`
    - this should include calls to any methods that handle sending the resulting messages to the message bus
- `_send_result_message`
    - This method is included in the template as an example on how to send a message to the message bus. It should be replaced with the component specific implementation.

More information about what these methods should do is included in the component template and in the general documentation about the simulation tools package: [https://github.com/simcesplatform/simulation-tools/tree/master#general-instructions-for-creating-a-new-simulation-component](https://github.com/simcesplatform/simulation-tools/tree/master#general-instructions-for-creating-a-new-simulation-component).
