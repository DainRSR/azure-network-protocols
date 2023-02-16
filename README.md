<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04


<h2>Actions and Observations</h2>

<p>
<img src="https://i.imgur.com/2YsW98v.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
1. First we are going to open our Azure subscription account. Go to Resources and create a new resource group. You can name it whatever you decide or you can follow what I have on my screen. Next choose the region you want. Make sure you remember the region because when we create the VM’s the regions need to match. 
</p>
<br />

<p>
<img src="https://i.imgur.com/c0AIdrm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
2. Next we will create our Virtual Machines. Go to the search bar and the virtual and it should appear. Go to create new and azure virtual machine. Select the resource group we just created and name your Virtual Machine  like ‘VM1’ or whatever you choose. Make sure the region matches the same for your resource group. Go to IMAGE and select WINDOWS 10. ** CHECK YOU LR REGION AGAIN, IT MAY HAVE CHANGED. JUST SELECT THE REGION THAT MATCHES THE RESOURCE GROUP** .  Go down to SIZE, you may want to select one with 2 or more virtual CPU’s and 16 GiB memory. This will allow the VM to run smoothly. Create a user name and password (make sure to save them for later use).  Check the box at the bottom to confirm. If you click next and go to Networking you will see the Vnet and Subnet have already been created. Click review + create.
3. Let’s create our 2nd virtual machine. Follow the same steps as step one. Add to the same Resource group. This time for IMAGE you will select UBUNTU SERVER 20.04 . Choose a username and password (remember it for later) . Click review + Create . After completion, Checkout your resource group and just make sure all your resource regions match and make sure you have 2 of everything matching except there will only be 1 virtual network that is attached to both VM’s. 
</p>
<br />

<p>
<img src="https://i.imgur.com/YgPZdh0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
4. Next go to search bar and type Network Watcher and on the left under Monitoring, select Topology . On the next page select the resource group and virtual network to see the topology of the network. This is important so that we can see the flow of traffic and how our system is connected. User to VM, NSG & IP to NIC to Vnet.
</p>
<br />

<p>
<img src="https://i.imgur.com/l76QiG2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
5. This step we will Remote Desktop into our VM’s and observe traffic. If you are a Windows computer then you should already have Remote Desktop, but for Mac users like myself we will have to down Microsoft Remote Desktop from the App Store first. After you download it press “command + spacebar” and type in Microsoft remote and it should appear . Open it and click add PC. Go to your VM1 and copy the IP address and paste it into your Remote Desktop and enter the username and password you created for the VM.  Your VM will load up and when you get to the privacy setting page just select NO for everything and click accept. When the networks screen appears click yes.  On MAC you can use your 3 fingers to swipe from VM to your regular desktop and back and forth as needed. 
</p>
<br />

<p>
<img src="https://i.imgur.com/0gM111K.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
6. Now we will download and install WireShark in our VM. Open your Microsoft edge browser. Go to google and Type WireShark. Download for windows and install. 
</p>
<br />

<p>
<img src="https://i.imgur.com/4sFcMFj.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
7. Go to the start menu and select WireShark . When you open Wireshark select ‘Ethernet’ and click the little blue shark fin, this will allow us to see what kind of traffic is flowing through our network. As you can see there is a lot going on but mostly in the background. We can see the public and private source IP address, destination and protocol. In the search bar we will type ICMP (internet control message protocol). Nothing is happening and that is because ICMP is like our ping command. When we ping it then we will see the traffic attempts.
8. Go back to Azure and open your VM2 so we can copy and paste the private IP address. Go back to VM1 and go to start menu and open Windows PowerShell. We will ping the private IP address for the Ubuntu VM. Type “ping “ and whatever private IP address you have. Watch how the traffic begins to move because VM1 is making contact with VM2 . Same network. You can see 4 packets were received and 4 were sent and zero were lost from VM2. Now we can go back to Azure and ALLOW ICMP traffic again. Pretty much we are switching the rule to allow instead of deny and then we can observe what happens to the traffic in our VM on WireShark. Now you can see our replies came back because we allowed ICMP through VM2’s firewall. You can stop traffic by pressing command C. 
</p>
<br />

<p>
<img src="https://i.imgur.com/SfdKkPt.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
9. Now let’s observe SSH traffic. (tcp-port == 22)We will Remote Desktop from VM1 to VM2 through ssh Using PowerShell we will type ssh (space) and the username you created for VM2 followed by @ and then put the private IP address of VM2. But enter and watch how very little traffic ensues in WireShark. (If you look at what the traffic says in the info section you’ll see words like: Ellipitic Curve Difficult-Hellman, that is part of Cryptography 🔐 it’s a key agreement protocol that allows two parties with both public and private keys to exchange information back and forth. This dives into the realm of cyber security**) Go back to PowerShell and see it says “Are you sure you want to continue connecting” just type Yes and see how traffic continues in ssh on Wireshark. PowerShell will print you to enter the password you created for VM2 (remember I said write it down for later). When you enter the password you will notice that you won’t see any characters appear on the screen, don’t be alarmed it’s normal. Just enter password and press enter. You will see you are now the VM2 user which is our Linux Ubuntu machine. You can type Linux commands like: id / uname -a  will tell us about the Operating system that’s running and see how traffic is filtered on Wireshark. The VM2 will interact with VM1 . Anything we type with go back and forth across the networks. Commands like ls -lasth will list current folders in the directory, but don’t mind those for now, we are just practicing observing the flow of network traffic . We will exit ssh now. Type in “exit” and press enter and now we are back to our VM1. 

</p>
<br />

<p>
<img src="https://i.imgur.com/C7UoxUM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
10. Now let’s look at some DHCP traffic. DHCP is a protocol that assigns IP addresses. Type DHCP in WireShark, then go to powershell and type ipconfig/renew . We will see if we can get our IP address reissued to us. Also the command ipconfig will let you see your IP address, subnet, gateway and local IPv6 address as well. 
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
11. Next we will filter some DNS traffic. Type DNS in WireShark and press enter. You’ll see DNS traffic already popping up. DNS is on port 53 and it gives names to ip address and gives up addresses names. Like if we go to our command line and type “nslookup www.google.com”  and we can see traffic start to move. We see the IP address for google, or at least ONE of their IP addresses. You can do the same method for www.disney.com  and see multiple IP addresses appear. 
</p>
<br />

<p>
<img src="https://i.imgur.com/NxDtjKZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
12. Lastly we will look at RDP traffic which is port 3389. Go to WireShark and type RDP or tcp.port == 3389 and press enter. ITS SPAMMING TRAFFIC LIKE CRAZY! That’s because we are on RDP for this lab connected to our VM. There’s lots of 3 way handshakes going on! (Look that up 😉).
</p>
<br />

<p>
<img src="https://i.imgur.com/prQiX2V.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
13. That concludes the lab! Feel free to practice this lab a couple times. And please don’t forget to DELETE your resource group in Azure. We don’t want to leave our VM’s running because it will slowly deplete you free subscription funds. 

</p>
<br />
