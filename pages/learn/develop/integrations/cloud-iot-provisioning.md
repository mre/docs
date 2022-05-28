---
title: Cloud IoT provisioning
excerpt: Get started with cloud IoT provisioning and {{ $names.company.lower }}
thumbnail: /img/integrations/aws/aws_iot_logo.png
---

The [AWS IoT](https://aws.amazon.com/iot) platform provides a suite of services to collect, store, and distribute IoT data. Its [IoT Core](https://aws.amazon.com/iot-core) service provides registration and messaging infrastructure for Internet-connected things like balena devices. At balena our goal is to reduce friction for fleet owners. So our IoT provisioning tool registers devices for you, and leverages balenaCloud and environment variables to store and access this registration data.

This guide provides an overview of IoT provisioning and gets you started with the [aws-iot-provision tool](https://github.com/balena-io-examples/aws-iot-provision). This guide is specific to AWS, but the integrations for other cloud providers work similarly.

## How It Works

![summary](/img/integrations/iot-overview.png)

Provisioning includes three components:

* **Service Container** like [Cloud Relay block](https://github.com/balena-io-examples/cloud-relay) on a device to request the provisioning and use the credential environment variables from balenaCloud (see note below)
* AWS Lambda **Cloud Function** to validate device identity and register the device with IoT Core
* **balenaCloud** to accept and store the generated key/certificate credentials for the device

The cloud function validates the device UUID in the provision request, generates an X.509 certificate and registers with the IoT Core service. The function then provides the generated credentials to balenaCloud, which stores and pushes them to the device as environment variables for use by the service container.

__Note:__ [Cloud Relay block](https://github.com/balena-io-examples/cloud-relay) makes it easy to send data to AWS IoT because it manages this provisioning for you and integrates with balena's block ecosystem for application development. You only need to send your data to an MQTT container on the device.

A service container like Cloud Relay on the device is not *required* to send the provisioning request. You may call the cloud function HTTP endpoint from your compute infrastucture to pre-generate the key/certificate for the cloud. However, the device must already exist in balenaCloud.


## Getting Started

The tools described here automate per-device integration with AWS IoT. However, first you must complete some initial one-time configuration on your AWS account. Sign up for an AWS account or log into your account at the [AWS Console](https://aws.amazon.com/). Once logged in, the [AWS IoT console](https://console.aws.amazon.com/iot/home) will show the IoT Things as you create them. See the [AWS IoT Core setup](https://github.com/balena-io-examples/aws-iot-provision#aws-iot-core-setup) section of the provisioning tool documentation for details.

## Create, Deploy, and Test Lambda function

The Lambda function provides a `provision` HTTP endpoint you use to request provisioning of a device, which requires its UUID. The provisioning tool [documentation](https://github.com/balena-io-examples/aws-iot-provision) walks you through creation of this Lambda function, including:

* setup of the node-lambda tool for development and deployment
* testing the function locally
* deployment to AWS and testing

The end result is a functioning HTTP endpoint on the AWS cloud, ready for provisioning requests.
