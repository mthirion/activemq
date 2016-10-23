# activemq cluster

## Compatibility
activeMQ 5.11 (from Red Hat JBoss A-MQ 6 extras)

## Description
This is an activeMQ architecture for scalability.
The architecture consists here in 4 brokers configured as a network of brokers.
We used unidirectional network links.

Potentially a provider could write to any of the available broker.
A consumer could also read from any broker, but it's recommended that the consumers read from one broker only.

![activemq cluster of 4 members](https://github.com/mthirion/activemq.git/cluster/brokers-flat4bis.png)


## Testing
Use the activemq-admin tool from the bin directory of any broker

Example:
Produce a message to brokerA:
activemq-admin producer --brokerUrl tcp://localhost:61616 --user admin --password admin --destination queue://myqueue --message "flowing though" --messageCount 2

Consume a message from brokerD:
activemq-admin consumer --brokerUrl tcp://localhost:61619 --user admin --password admin --destination queue://myqueue

## Scalability
Adding one more broker requires some configuration (network of broker config) on 2 brokers: the new one and the one it will be linked to.

# Production use
For persistent messages in production, each of the brokers should have at least one slave.
For a maximum of high availability, we'd use 3 brokers (instead of 4 described above) over 3 nodes; each broker would have 2 slaves located on another VM.
With that configuration, the MQ backbone still works under a situation where a failing infrastructure is reduced to one single working VM out of 3.

![activemq cluster : 3 members HA ](https://github.com/mthirion/activemq.git/cluster/brokers-flat4ha.png)

