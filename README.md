<h1>Palo Alto Radius Authentication</h1>


<h2>TLDR Description</h2>
Configuring RADIUS to authenticate logins for Palo Alto firewall.
<br />

<h2>Purpose</h2>
The purpose of this lab is to configure Radius Authentication using Windows Server 2019 to authenticate logins for the Palo Alto 220 Firewall GUI.
<br />

<h2>Background Info</h2>
RADIUS or Remote Authentication Dial-In User Service is a service which allows for AAA (authentication, authorization, and accounting) management and it was first introduced in 1991. The basic idea of RADIUS authentication is that the radius client sends a authentication request to the server which holds the database of accounting, the RADIUS server then replies with one of 3 things, access reject, access challenge, access accept. Access reject and accept are exactly how they sound, within these replies the RADIUS server either rejects or accepts the request sent by the client. Access challenge is when the RADIUS server replies with a request back to the client for more information such as a secondary access password.
<br />

<h2>Lab Summary</h2>
I started with the configuration of the windows server by first configuring active directory for use, I created a user and a security group named palo and put the created user in the security group for my radius auth. I then configured the NPAS side of my windows server and configured my palo alto firewall as a radius client to my windows server, configured the connection and network policies to allow for connections between the firewall and the radius server and set PAP as the authentication method and for the palo alto connection to pull from our Palo Alto security group we configured at the beginning. I then moved on to the configurations on the Palo Alto side, which was essentially just setting the windows server as the main radius server and setting the authentication sequence configurations to draw from the radius server and the specific security group we configured within the windows server. To test my configurations I created an example user with a password set on my windows server and then created an administrator profile on the palo alto side which was configured to verify the user’s credentials through the radius server rather than the local user database. 
<br />

<h2>Network Diagram</h2>
<p align="center">
  <img src="https://i.imgur.com/BAHHgtr.png" height="40%" width="40%" alt="Network Diagram"/>
</p>

<h2>Walk-Through:</h2>
<br/>
<h4>Configurations on Windows Server:</h4>

<p align="center">
Creating the user security group that our user will be a part of.<br/>
<img src="https://i.imgur.com/rx5wCNM.png" height="50%" width="50%" alt="Palo Alto RADIUS Authentication"/>
<br />
<br />
Creating the user that we will use to log into the firewall.<br/>
<img src="https://i.imgur.com/82waaL6.png" height="65%" width="65%" alt="Palo Alto RADIUS Authentication"/>
<br />
<br />
Adding the created user to the security group we created in the first step.<br/>
<img src="https://i.imgur.com/KHEFlkL.png" height="50%" width="50%" alt="Palo Alto RADIUS Authentication"/>
<br />
<br />
Adding the palo alto (192.168.1.1 internal address) as a radius client.<br/>
<img src="https://i.imgur.com/IARpYPZ.png" height="60%" width="60%" alt="Palo Alto RADIUS Authentication"/>
<br />
<br />
Configuring the connection policies, allowing for traffic from the palo alto firewall to reach our windows radius server.<br/>
<img src="https://i.imgur.com/lioL7Tc.png" height="65%" width="65%" alt="Palo Alto RADIUS Authentication"/>
<br />
<img src="https://i.imgur.com/uBaIhhA.png" height="65%" width="65%" alt="Palo Alto RADIUS Authentication"/>
<br />
<br />
Configuring network policies.<br/>
<img src="https://i.imgur.com/rzmhwhk.png" height="65%" width="65%" alt="Palo Alto RADIUS Authentication"/>
<br />
<br />
Pull the radius requests from the specified security group we created in the first step.<br/>
<img src="https://i.imgur.com/YWSFbFJ.png" height="65%" width="65%" alt="Palo Alto RADIUS Authentication"/>
<br />
<br />
Configuring the radius server to authenticate with PAP.<br/>
<img src="https://i.imgur.com/oN1bNAF.png" height="65%" width="65%" alt="Palo Alto RADIUS Authentication"/>
<br />
<br />
Adding a vendor specific attribute to our network policy.<br/>
<img src="https://i.imgur.com/vaFaIXO.png" height="65%" width="65%" alt="Palo Alto RADIUS Authentication"/>
<br />
<br />
Configuring vendor specific to fit palo alto configurations.<br/>
<img src="https://i.imgur.com/c95qdQf.png" height="60%" width="60%" alt="Palo Alto RADIUS Authentication"/>
<br/>
<img src="https://i.imgur.com/9EzXnta.png" height="60%" width="60%" alt="Palo Alto RADIUS Authentication"/>
<br/>
<img src="https://i.imgur.com/PGasznI.png" height="60%" width="60%" alt="Palo Alto RADIUS Authentication"/>
<br />
<br />
<br/>
<h4>Configurations on Palo Alto Firewall:</h4>
<p align="center">
Device > Admin Roles > Add<br/>
<img src="https://i.imgur.com/a6PUCpp.png" height="80%" width="80%" alt="Palo Alto RADIUS Authentication"/>
<br />
<br />
Configuring admin role<br/>
<img src="https://i.imgur.com/AOJBJi6.png" height="60%" width="60%" alt="Palo Alto RADIUS Authentication"/>
<br />
<br />
Device > Server Profile > RADIUS > Add<br/>
<img src="https://i.imgur.com/NQPcui1.png" height="80%" width="80%" alt="Palo Alto RADIUS Authentication"/>
<br />
<br />
Configuring the palo alto to pull from our windows radius server at 192.168.1.6<br/>
<img src="https://i.imgur.com/N9bSIHs.png" height="60%" width="60%" alt="Palo Alto RADIUS Authentication"/>
<br />
<br />
Device > Authentication Profile > Add<br/>
<img src="https://i.imgur.com/Bmuejs0.png" height="80%" width="80%" alt="Palo Alto RADIUS Authentication"/>
<br />
<br />
Configuring authentication for authentication from local palo alto database<br/>
<img src="https://i.imgur.com/Ee17UpX.png" height="60%" width="60%" alt="Palo Alto RADIUS Authentication"/>
<br />
<img src="https://i.imgur.com/ogp8LjO.png" height="60%" width="60%" alt="Palo Alto RADIUS Authentication"/>
<br />
<br />
Configuring the authentication profile which pulls from our specified directory on our windows radius server.<br/>
<img src="https://i.imgur.com/uzVjIU7.png" height="60%" width="60%" alt="Palo Alto RADIUS Authentication"/>
<br />
<img src="https://i.imgur.com/sW4K3dP.png" height="60%" width="60%" alt="Palo Alto RADIUS Authentication"/>
<br />
<br />
Device > Authentication Sequence > Add<br/>
<img src="https://i.imgur.com/W1mZjwm.png" height="80%" width="80%" alt="Palo Alto RADIUS Authentication"/>
<br />
<br />
Configuring our Authentication Sequence to verify through our radius authentication prior to attempting to authenticate with the local database on the Palo Alto.<br/>
<img src="https://i.imgur.com/47L8v0T.png" height="60%" width="60%" alt="Palo Alto RADIUS Authentication"/>
<br />
<br />
Device > Setup > Management > Authentication Settings > edit<br/>
<img src="https://i.imgur.com/u1YTki5.png" height="80%" width="80%" alt="Palo Alto RADIUS Authentication"/>
<br />
<br />
Configuring the Authentication Settings to use the Authentication Sequence we created.<br/>
<img src="https://i.imgur.com/KFNxYDC.png" height="60%" width="60%" alt="Palo Alto RADIUS Authentication"/>
<br />
<br />
Creating admin profile so that palo alto knows which user to look for in our directory on our windows radius server.<br/>
<img src="https://i.imgur.com/QQgOhnX.png" height="60%" width="60%" alt="Palo Alto RADIUS Authentication"/>
<br />
<br />
</p>
<h4>Proof of Functionality:</h4>
<br/>
<p align="center">
From the Palo alto system logs we can see that it authorized our login using the user created on the Radius Windows Server at 192.168.1.6<br/>
<img src="https://i.imgur.com/ugSnSVl.png" height="65%" width="65%" alt="Palo Alto RADIUS Authentication"/>
<br />
<br />
From the side of our Radius server by using event viewer we can see that the palo alto requested auth and the NPS (our Windows Radius Server) granted access to the credentials presented.<br/>
<img src="https://i.imgur.com/hjk7myO.png" height="70%" width="70%" alt="Palo Alto RADIUS Authentication"/>
<br />
<br />
In Wireshark we are able to see that our users password is encrypted.<br/>
<img src="https://i.imgur.com/ZFvb7o1.png" height="65%" width="65%" alt="Palo Alto RADIUS Authentication"/>
<br />
<br />
</p>

  
</p>

<h2>Problems</h2>
When configuring the use of a common authentication protocol I attempted to use MSCHAP provided by the Microsoft server which for some reason did not seem to be supported by the palo alto firewalls GUI, I substituted the use of MSCHAP for PAP and selected an option to encrypt sensitive user data which was shown in the wireshark screenshot provided in the configurations section. It worked fine as PAP standard was supported by both Palo Alto and Microsoft server.
<br/>
<br/>


<h2>Conclusion</h2>
In this lab I generated certificates to very legitimate user access through the global protect remote access VPN I configured through the Palo Alto 220 firewall. There were a lot of new concepts in this lab that I had to research to find out more information about, I think the biggest concepts that I faced were the certificate creation, use of them, and the configuration of the global protect gateway which was finally configured after extensive research and collaboration with the other members in the lab.
<br />

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
