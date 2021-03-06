# Overview for developer

## Basic concepts

- [Time and synchronization with epochs](core_time.md)
- [Platform managed and externally managed components](core_cmp-mgmt.md)
- [Service-oriented architecture](core_soa.md)

## How to develop

- [Creating new component](core_create-cmp.md)
    - [Building Docker image for a component](core_docker-image.md)
    - [Creating a component manifest file](core_component-manifest.md)
- [Conventions of messaging](core_conv-msg.md)
- [Conventions of naming](core_conv-name.md)
- [Pseudocode reference](core_pseudocode.md)
- [Simulation Tools package](core_sim-tools.md)
    - [AbstractSimulationComponent](core_abstractsimulationcomponent.md)
- [Workflow of component in simulation](core_workflow-sim.md)
- [Workflow of start and end](core_workflow-start-end.md)

## Communication

- [Topics, queues and exchanges](core_topics-queues-exchanges.md)
- [Exchange, management](core_exchange-mgmt.md)
- [Exchange, simulation-specific](core_exchange-sim.md)
- [Topics (core)](core_topics.md)
- [Reliable messaging in RabbitMQ](core_rabbitmq-reliability.md)
- [Footprint of AMQP items](core_amqp-footprint.md)

## Message structures (core)

- [Message structures (core)](core_msg.md)
- [Message type names (core)](core_msgtype.md)
- [Units of measure (UCUM)](core_ucum.md)
- [Message - AbstractMessage](core_msg-abstractmessage.md)
- [Message - AbstractResult](core_msg-abstractresult.md)
- [Message - Epoch](core_msg-epoch.md)
- [Message - SimState](core_msg-simstate.md)
- [Message - Start](core_msg-start.md)
- [Message - Status](core_msg-status.md)
- [Block - Quantity](core_block-quantity.md)
- [Block - Quantity array](core_block-quantity-array.md)
- [Block - Time series](core_block-time-series.md)

## Platforms internals

- [Logging system](core_logging-system.md)
- [LogWriter](core_logwriter.md)
- [Platform Manager](core_platformmanager.md)
- [Simulation lifecycle](core_lifecycle.md)
- [Simulation Manager](core_simulationmanager.md)
- [Dummy Component](core_dummycomponent.md)
- [AmqpMathToolIntegration](core_amqpmathtool.md)
