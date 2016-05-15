Inital Setup and Installation
==========================

Requirements
----------------
The .OVF is made for VMWARE ESX/ESXi 6.0.0+ but it might work for other Hypervisors as well.

* 4 Cpus
* 4 Gb Ram
* 200 Gb Disk (Prefferably SSD)

Default Credentials
------------

All Default credentials can be found in the /root/redsvgreen folder

* centos: root/redvsgreen
* Webinterface: admin/redvsgreen

VM Setup
------------

Installing is as simple as importing the .OVF template on the Hypervisor.

Verify that the requirements(cpu/mem etc) are met. Thin provisioning the harddrives is possible. It should not use more than 10gb of disk, but specify 200 GB so you have time to catch any issues.

Placement in the datacenter
------------

Example of two setups:

1. Directly in the OOB network.

.. code-block:: rst

      OOB-Network: 192.168.200.0/24
      redvsgreen-ip: 192.168.200.70

This allows you to monitor all equipment even if there is a router/firewall failure.

2. On a separate monitoring network

.. code-block:: rst

      OOB-Network: 192.168.200.0/24
      Monitoring-Network: 192.168.100.0/24
      redvsgreen-ip: 192.168.100.70

This allows for a more secure setup as protocols can be limited and traffic can be inspected by a intermediate device. If the intermediate device fails you loose your monitoring capabilites.

For uptime concers go with option 1, for security concerns go with options 2.

.. note:: As always the traffic to and from the monitoring device should be limited. Escpecially true if you are using it to store backups with oxidized which uses SSH. More on how to secure this is in the oxidized chapter.

Management IP
------------

After you have imported and started it head for the console access.

The default ip of RedvsGreen is 192.168.200.70, its unlikely this fits your needs so lets change it:

1. Login with root/redvsgreen.

You will be shown a prompt with the logo as well as some default settings.

2. Type "nmtui" in the cli prompt
3. Edit a connection
4. Choose your interface "eno16777736"
5. Edit it to fit your needs

.. note:: Make sure you add the correct dns settings, as it will become very slow if it cannot resolve dns names.

6. When you are done hit OK and exit the application
7. Now we need to restart it. Type:

      `shutdown -r now`

Verify that it works by accessing the device by ssh this time.


TimeZone
------------

Timezone is easily change by editing two files.

First we store the old timezone as a backup:

     `mv /etc/localtime /etc/localtime.bak` 

It will prompt you to overwrite it, this is ok.

As an example we list all available timezones in europe:

     `ls -al /usr/share/zoneinfo/Europe/`

Then we make a symlink to our chosen timezone:

     `ln -s /usr/share/zoneinfo/Europe/Stockholm /etc/localtime`

Verify the new timezone by typing:

     `date`

Second file we need to edit is the php.ini:

       `vi /etc/php.ini`

When the file is open type "/timezone" to search for it.
Change the line to reflect your needs. Example

       `date.timezone = Europe/Stockholm`
       
Restart the httpd deamon to make use the new timezone:

       `systemctl restart httpd`  

NTP
------------

.. warning:: Alot of plugins and tools rely on time being exact to the second. Do not proceed without syncing to a ntp server.

Edit ntp.conf:

      `vi /etc/ntp.conf`

Change the line "server 192.168.206.5 iburst" to fit your needs.

      server ntp1.example.com iburst
      server ntp2.example.com iburst
      
After you have made the change restart the server:

    `shutdown -r now`

When it boots up verify that time is synced:

      `ntpstat`
      
Example output:

    `synchronised to NTP server (192.168.206.5) at stratum 3`  
    `time correct to within 8522 ms`  
    `polling server every 64s`  

Accessing the WebGui
------------

Webgui can be accessed on:

      `https://redvsgreenipaddress`
      
Default credentials are: admin/redvsgreen

Troubleshooting
------------

Cacti and dashinga2 stops working after timezone change:
~~~~~~~~~~~~~~~~~~~~

If you change the time backwards in time the poller will stop until you reach the old time you had. To fix this we need to force it to run once.

      `php /var/www/html/cacti/poller.php --force"
      
Run it for 10 seconds and then hit CTRL + C to quit.

You should also stop dashinga2 and start it from commandline:

      `systemctl stop dashinga2`
      
      `cd /usr/share/dashinga2`
      
      `dashing start`
      
      Run it for 10 seconds then stop it using CTRL + C
      
      `systemctl start dashinga2`
      



