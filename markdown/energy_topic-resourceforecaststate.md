# ResourceForecastState.(ResourceCategory).(ResourceId)

This topic uses subtopics to enable finer-grained message routing. The naming is as follows:

```nohighlight
ResourceForecastState.(ResourceCategory).(ResourceId)
```

The supported resource categories are specified in the page of the component [Resources](energy_resources.md).

For example:

```nohighlight
ResourceForecastState.Generator.Generator1
```

Please note that subtopics do not prevent any component from receiving messages from all subtopics, as there can be wildcard subscriptions.
