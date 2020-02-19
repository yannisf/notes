# Profile

1. Download and install VisualVM from https://visualvm.github.io
2. Start your java application with the following JVM paremeters:

```text
-Dcom.sun.management.jmxremote
-Dcom.sun.management.jmxremote.port=10999
-Dcom.sun.management.jmxremote.local.only=false
-Dcom.sun.management.jmxremote.authenticate=false
-Dcom.sun.management.jmxremote.ssl=false
-Djava.rmi.server.hostname=127.0.0.1
```

3. Connect from VisualVM to the Java application. This is feasible in two ways: 1. using `jstatd` 2. using `jmx`. Prefer `jmx` since it provides aditional metrics.
