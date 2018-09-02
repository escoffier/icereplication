This example demonstrates IceGrid's [registry replication][1] feature.

To run the demo, start the IceGrid registries and nodes (you can start
the registries and nodes in any order, you can also start one or two
registries and start more later):

```
icegridregistry --Ice.Config=config.master
icegridregistry --Ice.Config=config.replica1
icegridregistry --Ice.Config=config.replica2
icegridnode --Ice.Config=config.node1
icegridnode --Ice.Config=config.node2
```

In a separate window:

```
icegridadmin --Ice.Config=config.client -e "application add application.xml"
client
```

This will deploy the application described in the file `application.xml` and
start the client.

The client invokes the number of specified iterations with a given
delay on a well-known proxy configured to use per-request load
balancing. Each invocation on the proxy queries the IceGrid registry
locator to retrieve the endpoints of the object (you can set
`Ice.Trace.Location=1` to see the locator invocations).

While the client is running and invoking on the server, you can try to
shutdown some of the registries with the `registry shutdown`
icegridadmin command or from the IceGridGUI. You can of course
also try to kill them. As long as one IceGrid registry is still
running, the client will continue to work.

Similarly, you can shutdown or kill one of the servers or nodes, and
the client will continue to work.

Finally, the registries and nodes are configured to redirect their
standard error and output to files in their data directories named
`stderr.txt` and `stdout.txt`. You can view these files using the
icegridadmin `registry show` or `node show` commands or with the
IceGridGUI. The standard error files contain the registry and
node traces.

[1]: https://doc.zeroc.com/display/Ice37/Registry+Replication
