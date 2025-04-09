Here we will discuss how to manually setup and configure both VM's on the Host system 

First we need to find the Host adapter, to do that we will run ipconfig/all 

![your adapter should look like this, MAC is obfiscuated in the image](ipconfigofVMadapter-1.png)

Now once we figure out the name of the adapter, we will switch to the VM Box to configure it. 

Your attacker (kalivm) should look like this. 

![Host-Only Adapter config in kalivm](kalisettingsadapter.png)

And your Metasploitable 2 (victim) should look like this. 

![Host-Only Adapter config in meta2](msf2vmconf.png)

Now we will boot up both the vm's to test and see if they show up on the arp -a table in the host system. 

```sh
sudo arp -a
```

DHCP is known to cause issues if you already have a Hyper-V Virtual Ethernet Adapter. So to fix this, we will assign the IP's statically to avoid any issues with DHCP handling IP assignment. 

To handle this we will assign the IP's statically in Host system (win 11).

```sh
netsh interface ip set address name="ADAPTER_NAME" static IP_ADDRESS SUBNET_MASK
```

Also make sure that in the VM box Network Manager DHCP is set to "Disabled" and uncheck "Enable Server" in the same page. 

![Your Network Manager should look like this](dhcpdiabledinvm.png)

Now, we fireup both VM's and check the arp table in the host system to see if our VM's show up under the Host Adapter IP. 

![when you run arp -a on host, it should look like this and show the IP's and MAC's of both VM's](arp-atableinhost.png)


Now we ping both the VM's from the host to see if they respond. 

![As you can see both 192.168.56.10 the Kali VM attacker, and 192.168.56.20 the Metasploitable2 VM victim](pingvmsfromhost-1.png)


Now we check the connectivity from the Kali VM (attacker). 

![As per this image you can see that the Host 192.168.56.1 and the victim 192.168.56.20 successfully ping](VirtualBox_Kali_29_03_2025_22_52_27.png)


Now we check again to confirm. 

![this has the kali attacker pinging the host and the victim along with the arp table](VirtualBox_Kali_29_03_2025_22_53_11.png)


Now we check the connectivity from the Metasploitable2 victim VM  

![As you can see you can ping the host from the victim VM](VirtualBox_msfvm_29_03_2025_22_50_07.png)


Now, your homelab is all setup and good to go! :)



Troubleshooting -- 

In case, the arp -a table doesnt show the IP's of one or both VM's we can manually assign it to them. 

For that we will first check the conf file of network. 

```sh
cat /etc/network/interfaces
```




