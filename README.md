# open-iot
An open specification for Internet-of-Things devices and apps to use a
local-network service instead of a cloud provider.

# Why Open-IoT?

Today, many new devices are appearing with exciting remote access capabilities.
This includes remote control, data gathering, alerting, and the
like—conceptually-simple things built into powerful tools like remote-access
door locks, video-streaming doorbells, and self-programming thermostats.

At the same time, we face a landscape of increasing concerns over security and
privacy.  IoT devices carry the risk of exploitation as hacker jump-off points,
remote spying devices, or dangerous attack points.  A hacker with system access
to your thermostat can override your heating system and, if the built-in safety
devices fail, can cause a crack in a gas heating coil and flood your house with
deadly carbon monoxide; if the safety circuits hold, he can still disable your
heat, or run up your heating bill by running the heater when nobody's home
(based on the IoT occupancy sensors) and disabling the reporting.  Hackers can
listen to your conversations through voice-activated platforms, or spy on your
activities through Internet-connected Web cams.

Even without malicious hackers, some people may not want the service provider
to receive constant, real-time updates of their activities through an IoT
device which makes measurements, metrics, and graphs in that resolution for the
user.  Many users would be comfortable supplying summary metrics, such as the
total amount of heating and cooling energy used on a daily or monthly basis as
recorded by a thermostat, but not with supplying the real-time occupancy
statistics the thermostat uses to determine when the house is unoccupied.  With
cloud-based services, the user can see the status of his thermostat at any
time, and its complete history; yet he must surrender all of that information
to the cloud provider or else such tools aren't even possible.

Open-IoT describes a standard to address these problems by allowing local
hosting of IoT services and providing secure interfaces between IoT devices,
applications, and hubs.

# IoT Hubs

An IoT Hub is essentially an _armhf_ or _x86-64_ device running Docker under
Linux.

IoT Hubs provide a Web interface on port 443 under TLS.  The certificate is
locally-generated and self-signed; Hubs can generate new certificates upon
administrative request.

Any client to the IoT Hub—whether an IoT Device or an IoT Application—must
validate and cache the certificate.  If the certificate changes, the client
must require user intervention to affirm the change.

## IoT Services

An IoT Service runs under a Docker container on an IoT Hub.  The IoT Hub
accepts connections on port 443 (HTTPS) and proxies them to the appropriate
service.  As part of the standard interface, the connections proxy to
unencrypted HTTP over a UNIX domain socket at `/opt/iot/service/interface.sock`
in the IoT Service container; this is so the `/opt/iot/service/` path can map
to a unique path in the IoT Hub container, placing the socket in a known
location.

The IoT Service may connect out to the IoT Device, or the IoT Device may
connect as a client to the IoT Service.

# IoT Client Registration

An IoT Client Registration allows a client, such as a tablet, smart phone app,
or even an IoT Device itself, to register with an IoT Hub Service.  This allows
for remote access to the Service over IPv6 directly from the Internet.

IoT Client Registration exchanges TLS certificates as an identification
mechanism.  The client application generates a TLS key pair and provides a
certificate, and it accepts and registers the IoT Hub's certificate.  A client
is only allowed to connect to the Hub if it can identify itself with an
accepted certificate.

This strict client identification limits access to IoT Device and Hub Service
code:  if the client fails to authenticate, it can't interact with IoT Hub
Services; and if the IoT Device only interfaces through the IoT Hub Service,
then attacking the Device becomes impossible without first compromising the IoT
Hub to imitate the Service.

IoT Client Registration provides the Client with the public IPv6 address of the
IoT device for access over the Internet from anywhere.
