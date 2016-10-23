# activemq hub & spoke

## Compatibility
activeMQ 5.11 (from Red Hat JBoss A-MQ 6 extras)

## Description
This is an activeMQ architecture for scalability. <br>
The architecture is an Hub & Spoke.
The brokers are configured within a network of brokers for the messages to be routed through the HUB. <br>
We used "duplex" links between the "HUBs" and the "spoke" brokers, and those links are exclusively configured in the hub brokers. <br>
So, only one of the hub has to be configured for a new leave to be added to the backbone.

Within such an architecture, producers and consumers are attached only to leaves and never to any hubs. <br>
Notice that we used unidirectional links between the "hubs" to better control the transmission of messages and the bandwith in the backbone.

![activemq cluster of 4 members](https://github.com/mthirion/activemq/blob/master/hub/brokers-hub5.png)


## Testing
Use the activemq-admin tool from the bin directory of any broker

Example:<br>
Produce a message to brokerA: <br>
activemq-admin producer --brokerUrl tcp://localhost:61616 --user admin --password admin --destination queue://myqueue --message "flowing though" --messageCount 2

Consume a message from brokerF: <br>
activemq-admin consumer --brokerUrl tcp://localhost:61621 --user admin --password admin --destination queue://myqueue

## Scalability
Spokes can be plugged to any hubs; only the hub will have to be reconfigured with a new duplex link to integrate the new spoke. <br>
The architecture can also be extended with new hubs.

# Production use
For persistent messages in production, each of the brokers should have at least one slave. <br>

