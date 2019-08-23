Modified systemd example for debugging a [balena forums issue](https://forums.balena.io/t/docker-run-arguments/25356/).

To use bluetooth in the container with `bluetoothctl`, connect to the system DBUS, as per [our docs](https://www.balena.io/docs/learn/develop/runtime/#dbus-communication-with-host-os):

```shell
DBUS_SYSTEM_BUS_ADDRESS=unix:path=/host/run/dbus/system_bus_socket bluetoothctl
```
