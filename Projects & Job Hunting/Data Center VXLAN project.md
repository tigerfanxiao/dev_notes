# Project Background

Current data center network is using old VLAN technology which can not support VM Dynamic Migration.

Dynamic Migration: Migrate VM from one physical server to another physical but without impacting the service

# Process

Pre-sale phase

Analyze the configuration of the device in the network from Huawei, Cisco and Ericsson to estimate the complexity of network, which will support the decision of bid price.

Task:

1.  Use python to parse configuration of level 2 network, VLAN, interface and its remote interface, network traffic status
2.  Save the information as structured data in the SQLite database
3.  With the information we can draw the current network topology and find devices which closely coupled together. It means we need to migrate those device together at one night. This kind of information is very important for estimating project lifecycle and labor cost.

Implementation phase

1.  use `netmiko` python network library to automate download configuration info from the current network and update the database.
2.  use `ansible` to batch configure the new devices
3.  extend the data center level 2 network with VXLAN technology