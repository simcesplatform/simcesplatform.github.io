# ResourceState.(ResourceCategory).(ResourceId)

This topic uses subtopics to enable finer-grained message routing. The naming is as follows:

```nohighlight
ResourceState.(ResourceCategory).(ResourceId)
```

- The supported resource categories are specified in the page of the components [Resources](energy_resources.md).
- For a particular message, the ResourceId part MUST match SourceProcessId in the message!

Example:

```nohighlight
ResourceState.Generator.Generator1
```

Please note that subtopics do not prevent any component from receiving messages from all subtopics, as there can be wildcard subscriptions.
