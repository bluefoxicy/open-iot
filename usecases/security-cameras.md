# Security Cameras

A Security Camera could provide its Internet interface through an Open IoT Hub
rather than a cloud service or a public address on the camera itself.

# Camera Setup

## User steps

 * Ensure Bluetooth is enabled and the camera is in range
 * Open IoT Hub app
 * Select "Register Device"
 * Select the Camera from the list

The above automatically connects the Camera to the IoT Hub, assuming the user's
Smart Phone or Tablet contains an IoT Hub app already connected to the IoT Hub.

## Technical Process

The above user action carries out the following.

The Camera would initially interface with a Smart Phone app via Bluetooth.
This is a secure control method and viable for all IoT devices, but only at
short range.

The IoT Hub app connects to the Camera and initiates registration.  This
requests a CSR from the Camera.

The Camera responds by generating a unique TLS key pair and sending a CSR to
the IoT Hub app.  It also sends a list of Services required, including the
Docker container URI for each service.

The IoT Hub app, which is already registered with the IoT Hub, connects via
IPv6 and submits a Device Registration.  This submits the CSR and all Services
to the IoT hub.

The IoT Hub returns a certificate signed by its self-signed certificate.  It
also downloads, configures, and starts all required Services.  The IoT Hub
grants access to these services to the client identified by the certificate.

The IoT Hub app then sends the signed certificate, the IoT Hub certificate, and
and the IoT Hub IPv6 address to the Camera.

The Camera can now connect to the IoT Hub and use the Camera service.  It can
further configure and be configured by the Camera service.

# App Setup

## User Steps

 * Open the Camera App
 * Connect to the Camera via Bluetooth
 * Register with IoT
 * Open the IoT Hub App
 * Grant access to the services requested by the new Camera app

The Camera would instruct its corresponding Smart Phone app to connect to the IoT Hub and register itself.  
