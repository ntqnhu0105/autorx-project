---
title : "Create an IoT Device"
date : 2026-07-09
weight : 1
chapter : false
pre : " <b> 5.4.1 </b> "
---

### Create an IoT Device

In the AutoRx system, the **Vehicle Simulator** acts as a virtual vehicle that publishes telemetry data such as speed, engine temperature, battery voltage, and odometer readings to the AWS Cloud.

Before a device can securely connect to **AWS IoT Core**, you must create an **IoT Thing**. AWS then issues an **X.509 certificate** that authenticates the device when it communicates over the MQTT protocol.

In this section, you will:

- Create an AWS IoT Thing.
- Generate an X.509 certificate.
- Create an IoT policy.
- Attach the certificate and policy to the Thing.
- Prepare the required credentials for the Vehicle Simulator to connect to AWS IoT Core.

---

### Device Connection Architecture

Before telemetry data can be sent to the system, the **Vehicle Simulator** must establish a secure connection with **AWS IoT Core**.

AWS IoT Core uses an **X.509 certificate** to authenticate the device. After successful authentication, the Vehicle Simulator publishes telemetry data to the configured MQTT topics using the **MQTT** protocol.

The following diagram illustrates the connection between the Vehicle Simulator and AWS IoT Core.

![IoT Device Connection](/images/5-Workshop/5.4-IoT-Pipeline/5.4.1-create-iot-device/iot-device-connection.jpg)

## Create an AWS IoT Thing

Sign in to the **AWS Management Console**, search for **AWS IoT Core**, and open the service.

![iot-core](/images/5-Workshop/5.4-IoT-Pipeline/5.4.1-create-iot-device/iot-core.jpg)

From the navigation pane, choose:

```text
Manage
    └── All devices
            └── Things
```

Then choose **Create things**.

![things](/images/5-Workshop/5.4-IoT-Pipeline/5.4.1-create-iot-device/things.jpg)

Select **Create single thing** to create a new IoT device.

![single-thing](/images/5-Workshop/5.4-IoT-Pipeline/5.4.1-create-iot-device/create-single.jpg)

Enter the following information:

| Property | Value |
|----------|-------|
| Thing name | AutoRx-Vehicle-01 |

Leave the remaining settings at their default values.

Choose **Next**.

![thing-name](/images/5-Workshop/5.4-IoT-Pipeline/5.4.1-create-iot-device/thing-name.jpg)

---

## Generate an X.509 Certificate

AWS IoT Core uses an **X.509 certificate** to authenticate devices.

On the **Certificate** page, select:

```text
Auto-generate a new certificate
```

Then choose **Next**.

![certificate](/images/5-Workshop/5.4-IoT-Pipeline/5.4.1-create-iot-device/certificate.jpg)

AWS automatically generates the authentication files for the device.

Download all of the following files:

- Device certificate
- Public key file
- Private key file
- Amazon Root CA 1

These files are required for the Vehicle Simulator to establish an MQTT connection with AWS IoT Core.

{{% notice warning %}}
You cannot download the **Private Key** again after leaving this page. Store all certificate files in a secure location before continuing.
{{% /notice %}}

![download](/images/5-Workshop/5.4-IoT-Pipeline/5.4.1-create-iot-device/download-certificate.jpg)

---

## Create an IoT Policy

After downloading the certificate, choose **Create policy**.

![create-policy](/images/5-Workshop/5.4-IoT-Pipeline/5.4.1-create-iot-device/create-policy.jpg)

Enter the following information:

| Property | Value |
|----------|-------|
| Policy name | AutoRx-IoT-Policy |

Add a policy statement with the following values:

| Property | Value |
|----------|-------|
| Effect | Allow |
| Action | iot:* |
| Resource ARN | * |

Then choose **Create**.

![policy](/images/5-Workshop/5.4-IoT-Pipeline/5.4.1-create-iot-device/policy.jpg)

> In a production environment, you should restrict permissions to specific MQTT topics or resource ARNs. This workshop uses the `iot:*` permission to simplify the setup process.

---

## Attach the Policy to the Thing

After creating the policy, return to the setup wizard.

Select the policy you just created:

```text
AutoRx-IoT-Policy
```

Then choose **Next**.

AWS automatically performs the following actions:

- Attaches the certificate
- Attaches the policy
- Attaches the Thing

Finally, choose **Done** to complete the setup.

![attach](/images/5-Workshop/5.4-IoT-Pipeline/5.4.1-create-iot-device/attach-policy.jpg)

---

## Verify the Configuration

Return to:

```text
Manage
    └── Things
```

Select **AutoRx-Vehicle-01**.

Verify that:

- The Thing has been created successfully.
- The certificate status is **Active**.
- The **AutoRx-IoT-Policy** is attached.

![result](/images/5-Workshop/5.4-IoT-Pipeline/5.4.1-create-iot-device/result.jpg)

After completing these steps, the Vehicle Simulator has all the required credentials to connect securely to AWS IoT Core using the MQTT protocol.

In the next section, you will configure an **AWS IoT Rule** to forward telemetry data to **Amazon SQS**.