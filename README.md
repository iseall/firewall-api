## Fiewall Gateway Uranus

Fiewall gateway Uranus is a Linux firewalld central controller. In Greek mythology, Uranus king of gods. The firewall gateway is the Uranus for iptables.

## Features

- Full firewalld features
- Full D-BUS API convert to REST API.
- Based dbus remotely.
- HTTP restful API.
- Declarative API and Imperative API.
- Support HA (Based Kubernetes)
- Asynchronous batch interface (only add).
- Can control thousands of linux machine via firewall gateway remotely.
- Support change tempate of thousands of machine fastly.
- Support wrong operation backoff.
- Support delay command effect.
- Support iptables NAT ipset timer task.
- Support template switch (only enable db).
- Only HTTP Service (without store).

## TODO
- [X] Asynchronous batch process (Signal thread)
- [ ] Asynchronous batch process (Multi thread)
- [X] optional API on (v3 only)
- [X] security policy
- [X] Delay task
- [X] rpm spec
- [ ] UI
- [ ] Authtication.
- [ ] Based Kubernetes HA.
- [ ] Prometheus Metics.
- [ ] WAF SDK.
- [X] Deplyment on Kubernetes


## Deploy

To Compiling Uranus, execute following command:

```bash
git clone ..
make
```

To deploy Uranus on kubernetes, execute following command:

```
kubectl apply -f https://raw.githubusercontent.com/cylonchau/firewalld-gateway/main/deploy/deployment.yaml
```

To run Uranus on docker, execute following command:

```bash
docker run -d --rm  cylonchau/uranus
```

if you think update you dbus-daemon verion to lasest, can use `dbus.spec` make your package.


## Thanks libs
- [kubernetes workqueue](https://github.com/kubernetes/kubernetes)
- [klog](https://github.com/kubernetes/kubernetes)
- [godbus](https://github.com/godbus/dbus)
- [gin](https://github.com/gin-gonic/gin)
- [viper](https://github.com/spf13/viper)

## use

[HTTP API DOC](https://documenter.getpostman.com/view/12796679/UV5agGNr)

- v1 runtime resource.
- v2 permanent resource.
- v3 Asynchronous batck opreation.

## FAQ

### Why not use ssh or ansible tools.

Because D-Bus support remotely and firewalld implemented full D-Bus API, so we can batch manage iptables rules via firealld.

### How diffrence your project and other

firewall gateway implemented full dbus API convert to HTTP API, so can control thousands of machine via gateway. And ohter project update iptables via agent scripts. or only run on one machines.


### Is enable D-Bus remotely safe?

We can open D-Bus port only accpet gateway's IP, so is safed

default if you machine hacked, enable of disable D-Bus remote, it doesn't make any sense. Because hacker can run any command on your machine.

If you machine Is safe, so we can through open D-Bus port only accpet gateway's IP, so can management iptables rules via gateway and UI

For example

- The layer 1, you can add iptables rule restrict dbus tcp port.
- The layer 2, you can use dbus ACL restrict request.

To edit /etc/dbus-1/system.conf, example.

```xml
<policy context="default">
    <deny receive_path="/org/fedoraproject/FirewallD1" /> <!-- restrict all request -->
    <allow user="root" />
    <allow own="com.github.cylonchau.Uranus" /> <!-- allow uranus resiger to dbus-daemon -->
    <!-- if requseter is com.github.cylonchau.Uranus and request path is /org/fedoraproject/FirewallD1, then allow  -->
    <allow receive_sender="com.github.cylonchau.Uranus" receive_path="/org/fedoraproject/FirewallD1" />
</policy>
```

### How to output debug ?

```
-v 5 // full log
-v 4 // info log
-v 2 // no log
```