# Log API

Once you have run a simulation, you can access the messages published by your components with the log API.


## Get simulation information

To verify that a simulation has been executed, you can retrieve simulation information. Type this address into your web browser:

```nohighlight
http://localhost:8080/simulations
```

- This assumes that you run the simulation platform locally
- This will return the information of all simulation runs this far

In the returned markup, you can verify that a simulation has occurred by looking at StartTime and EndTime strings. These should have a time value that matches the time you executed the simulation.


## Detailed instructions

**TODO:** describe log API here!
