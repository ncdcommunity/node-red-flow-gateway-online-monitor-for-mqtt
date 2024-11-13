# node-red-flow-gateway-online-monitor
A Node-RED flow that monitors the internet connection on the Enterprise IIoT Gateway. If the internet connection is lost, sensor data is stored locally. Once the connection is restored, the stored data is sent.

Example flow:

![ncd-online-monitor-and-data](https://github.com/user-attachments/assets/fb3e4e9d-9485-45b0-a607-15491e4f551f)


## Nodes
### ncd-filter
![ncd-filter-node](https://github.com/user-attachments/assets/4a24cc2d-82e4-4db1-afcc-8a18446df633)

Description: A node that enables intuitive filtering of messages received from the Wireless Gateway node. This node can be configured to allow the flow of all messages, or to filter messages based on specific criteria. The following filtering options are available:

#### Mode

- **All messages (Sensors and Modem/Gateway)** – Allows all messages to pass through.

- **Only messages from sensors** – Filters and allows only messages originating from NCD sensors.

- **Only messages within the 'Sensor Data' property** – Filters messages based on the "Sensor Data" property.

- **Only sensor mode messages** – Filters and allows only messages of a specific type, such as RUN, FLY, ACK, etc.

- **Only messages from Gateway/Modem** – Allows only messages originating from the Gateway/Modem itself.

#### Sensor Type

Sensor type filtering – Enables filtering by sensor type (numerical value), allowing you to select messages from one or more sensors of the same type.

#### Mac Address

MAC address filtering – Filters messages based on the MAC address of the sensor, allowing only messages from a specific sensor to pass through.

#### Sensor Data Variables (Applicable only in messages within the 'Sensor Data' property)

Sensor Data Variables – Allows the flow of only specific field variables associated with the specified sensor.

This flexible configuration makes it easy to manage and control which data flows through the system, enhancing both performance and data accuracy.

### ncd-internet-monitor
![ncd-internet-monitor-node](https://github.com/user-attachments/assets/f1b9796c-b857-437c-80f4-9e89901aca32)

Description: A simple node that runs every second and attempts to establish a connection to an internet server. Based on the connection status, it sets the value of a variable that indicates whether the Enterprise IIoT Gateway has access to the internet. This variable can also be queried by the 'ncd-isonline' node(s) to determine their operation.

### ncd-isonline
![ncd-isonline-node](https://github.com/user-attachments/assets/1e21045f-7a05-4479-907d-5ec3f4c0fd22)

Description: This node evaluates the state of the connection. If an active internet connection is detected, incoming messages flow through to the outgoing terminal. However, if the connection is lost (offline), incoming messages are routed through nodes that create a local database on the Enterprise IIoT Gateway. When the internet connection is reestablished, incoming data resumes flowing through the node without modification. Simultaneously, the locally stored data is sent through a second terminal every 0.5 seconds. Once all the data has been transmitted, the local database on the Enterprise IIoT Gateway is deleted.






