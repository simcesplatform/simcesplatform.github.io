# Getting logged data

As soon as there has been communication between simulation components, you can access the published messages using the Log Reader API.

## Get simulation information

To verify that a simulation has been executed, you can retrieve simulation information. Type this address into your web browser:

```
http://localhost:8080/simulations
```

- This assumes that you run the simulation platform locally
- This will return the information of all simulation runs this far

In the returned markup, you can verify that a simulation has occurred by looking at StartTime and EndTime strings. These should have a time value that matches the time you executed the simulation.


## Detailed instructions
The full API documentation is available in the page Read/Export API. **TODO: link!**
