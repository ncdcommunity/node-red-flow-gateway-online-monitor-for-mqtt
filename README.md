# node-red-flow-gateway-online-monitor-for-mqtt
A Node-RED flow that monitors the successful transmission of messages via MQTT on the Enterprise IIoT gateway. If a message fails to send—due to internet connectivity loss or disconnection from the MQTT broker—the sensor data is saved locally or at a user-defined location. Once the connection to the broker is restored, the stored data is transmitted and removed from local storage.

> [!IMPORTANT]
> For the flow to function correctly, the Quality of Service (QoS) must be enabled in the MQTT nodes, with at least QoS level 1. Without this, the flow may not operate as intended.
> 
> ![ncd-gateway-online-monitor-for-mqtt-qos](https://github.com/user-attachments/assets/70e6de07-3569-4b61-b0f6-ccfce8f6f033)

## Overview
![ncd-online-monitor-for-mqtt-overview](https://github.com/user-attachments/assets/d4b47cbf-0c47-4dc2-a8e8-022735a501bd)


### Example flow:

![ncd-gateway-online-monitor-for-mqtt-example-flow](https://github.com/user-attachments/assets/6f2cacc0-dc36-4e92-ae91-99a3e0a05ec9)


## Main Nodes
### Set LogFile
![ncd-gateway-online-monitor-for-mqtt-set-log-node](https://github.com/user-attachments/assets/d533d31b-3ddc-4301-8967-f8e7d0965104)

**Description:** This node allows you to specify the path or location where data will be stored locally on the Enterprise IIoT Gateway. By default, this property is set to the Node-RED path. The node is automatically executed each time a flow is deployed.


### Complete
![ncd-gateway-online-monitor-for-mqtt-complete](https://github.com/user-attachments/assets/c51d8ec6-d5f2-4799-82c7-db89d289398f)

**Description:** This node triggers a flow when another node has completed its message handling. It can be associated with one or more nodes; in this case, it must be linked to the MQTT Out node. The node triggers a flow message once it has successfully sent data via MQTT. (It is important that the MQTT node has QoS enabled, with at least QoS level 1 or 2.)


### Prepare Data
![ncd-gateway-online-monitor-for-mqtt-prepare-data-node](https://github.com/user-attachments/assets/598b2a8f-9a75-483e-98db-cdc200fc19ce)

**Description:** This node creates an array of message identifiers, 'msg._msgid' (a unique identifier automatically assigned to each message by Node-RED, useful for tracking and debugging messages within the flow). Using an algorithm and the complete node, it becomes possible to determine whether a message was successfully sent via MQTT.


### Get path & Read first line form backlogs
![ncd-gateway-online-monitor-for-mqtt-get-path-and-read](https://github.com/user-attachments/assets/323c0f46-fdb9-4f5e-a12d-1ea6f476a588)

**Description:** These nodes are executed automatically as part of a flow that periodically checks for data in the backlog file (or the path specified by the user). When data is found in the backlog file, it is passed to the main flow, which attempts to send the data via MQTT.


### If Message is still pending send to backlog
![ncd-gateway-online-monitor-for-mqtt-pending-algorithm](https://github.com/user-attachments/assets/36f153b1-b20d-48e2-a2eb-7a37b866d230)

**Description:** This node works in conjunction with the complete and prepare node and contains an algorithm that evaluates whether a message attempted to be sent via MQTT was successfully delivered or if it failed to be completed within a defined timeout period.

If a message is not successfully completed, the node sends it through output number one, allowing the flow to add the unsent message to the backlog. If the message was successfully sent but originated from the backlog (msg.befrom), it is routed to the node’s output number two to remove it from the backlog.


---
© 2019-2024 National Control Devices, LLC. All rights reserved.


