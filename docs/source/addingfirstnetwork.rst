Adding your first devices
=========================================================

This section describes how to scan your first network and adding the devices to RedvsGreen.

.. Tip:: Before proceding it can be good to standardize on a common snmp standard for all devices. Example SNMP V3.

Cacti Automation Network Scanning
------------

Login in to RedvsGreen via the Webgui and Click on the cacti button.

First we need to setup the default snmp credentials for your network.

    Go to: Console -> Automation -> SNMP Options
    
Open the default option set.

.. Note:: Supports SNMP v 1,2,3

Add all your SNMP credentials.

After you filled out all of your credentials:

    Go to: Console -> Automation -> Networks
    
Open "Test Network" and change it to reflect the network you will scan. After you changed it hit the save button and go back to the previous page. Here you can select "Test network" and manually issue a "Discover Now" event.


.. Tip:: You can reuse the "Test Network" for other scans, or you can create your own network and have them continuasly scanned for automatic device addition.

.. Note:: All devices needs to be reachable by ICMP ping and SNMP(UDP 161)

Primary installation
~~~~~~~~~~~~~~~~~~~~

test
