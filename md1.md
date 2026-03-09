# EntraID 

## Group

Create a VPN group

Assign users to the VPN group

## Application

Create the Enterprise Application in EntraID, under “Enterpise Apps”
click “+ New Application” and search for “FortiGate” select the
“FortiGate SSL VPN”

![image 1](./media/image1.png)

Give the App a name

<img src="media/image2.png"
style="width:3.75962in;height:3.42747in" />

Under “Single sign-on” on the left menu, select “SAML”

<img src="media/image3.png"
style="width:6.08737in;height:2.05497in" />

Click “Edit” on the “Basic SAML Configuration”

<img src="media/image4.png"
style="width:4.71926in;height:1.17537in" />

<img src="media/image5.png"
style="width:6.26806in;height:5.13472in" />

In here we need to “add identifier” this should look like (note the HTTP
and not HTTPS):

Patterns: <https://*.FORTIGATE-FQDN.com/remote/saml/metadata> e.g.

<http://92.42.120.2:1001/remote/saml/metadata>/ or
<http://vpn.domain.com:1001/remote/saml/metadata>/

Make sure you include the “/” at the end of the line.

The add the “reply URL”

Patterns: https://\<FORTIGATE-FQDN\>/remote/saml/login e.g.

<https://92.42.120.2:1001/remote/saml/login> or
<https://vpn.domain.com:1001/remote/saml/login>

Now add the “Sign on URL”

Patterns: https://\<FORTIGATE-FQDN\>/remote/saml/login e.g.

<https://92.42.120.2:1001/remote/saml/login> or
<https://vpn.domain.com:1001/remote/saml/login>

Lastly on this page add the “Logout URL (Optional)

Pattern: https://\<FORTIGATE-FQDN\>/remote/saml/logout e.g.

<https://92.42.120.2:1001/remote/saml/logout> or
<https://vpn.domain.com:1001/remote/saml/logout>

Now click the “Save” button at the top of the page. The “Basic SAML
Configuration” should now look something like this:

<img src="media/image6.png"
style="width:6.26806in;height:1.58542in" />

Now click “Edit” on “Attributes & Claims”

<img src="media/image7.png"
style="width:6.26806in;height:1.67569in" />

Click “Add new claim”

Name: username

Source attribute: user.userprincipalname

<img src="media/image8.png"
style="width:6.26806in;height:1.48542in" />

Now click “Save”

Now if a Claim exists called “Group” delete this

<img src="media/image9.png"
style="width:6.26806in;height:0.53958in" />

<img src="media/image10.png"
style="width:3.30254in;height:1.25017in" />

This will Error but will remove the group claim

<img src="media/image11.png"
style="width:3.7401in;height:1.05223in" />

and the “Add a group claim”, select the “Security groups” then tick
“Customize the name of the group claim” and set the name to “group” and
save.

Now under SAML Certificates, click “Edit”

<img src="media/image12.png"
style="width:5.11812in;height:2.55566in" />

Change the “Signing Option” from “Sign SAML assertion” to “Sign SAML
response and assertion” and click “Save”

<img src="//media/image13.png"
style="width:4.46588in;height:1.98159in" />

Now download the Certificate (Base64)

<img src="media/image14.png"
style="width:2.50716in;height:0.24788in" />

Under “Security” click “Condtional Access” and select “New Policy”

Give the policy a name e.g. “FortGate VPN MFA” and then under “Access
Controls” click “Grant” and set the MFA as desired.

Under “Users or agents” choose “Select Users and groups” and tick “Users
and groups” then select either the VPN group or each user

<img src="media/image15.png"
style="width:4.78319in;height:5.03332in" />

<img src="media/image16.png"
style="width:1.83492in;height:2.87411in" />

On “Enable Policy” make sure this is set to “On” and click “Create”

<img src="media/image17.png"
style="width:1.59792in;height:0.64422in" />

Under “Manage” – “Users and Groups” that the VPN group is added here

<img src="media/image18.png"
style="width:6.26806in;height:1.46597in" />

# FortiGate

## FortiGate Certificate

Under “System” – “Certificates” the click “Create/Import”

<img src="media/image19.png"
style="width:1.42723in;height:1.89491in" />

Click “Use Let’s Encrypt” and follow the wizard.

<img src="media/image20.png"
style="width:6.26806in;height:2.77431in" />

<img src="media/image21.png"
style="width:6.26806in;height:1.80139in" />

## Remote Certificate

Download the certificate from the created application this should be the
“Certificate (Base64)”

<img src="media/image22.png"
style="width:4.66915in;height:1.6864in" />

On the FortiGate in “System” – “Certificates” click “+ Create/Import”
and select “Remote Certificate”

<img src="media/image23.png"
style="width:1.3822in;height:1.63561in" />

Now click on “Upload” and locate the downloaded certificate

<img src="media/image24.png"
style="width:2.36491in;height:1.19808in" />

Once uploaded you can either leave the default certificate name or
rename this certificate as below so it can be easily identified

<img src="media/image25.png"
style="width:6.26806in;height:3.42708in" />

## Create Single Sign-on

Under “User & Authentication” select “Single Sign-On” and select “+
Create New”

<img src="media/image26.png"
style="width:6.26806in;height:3.55278in" />

<img src="media/image27.png"
style="width:6.26806in;height:4.82431in" />

## User Groups

To create the user groups go to “User & Authentication” and select “User
Groups” and click “+ Create New”

<img src="media/image28.png"
style="width:6.26806in;height:3.84931in" />

On the “Add Group Match” Remote server is the one we just created and
the Groups should be “Specify” and insert the Group object ID from the
EntraID groups page.

<img src="media/image29.png"
style="width:4.70899in;height:1.72941in" />

## Create IPSEC VPN

Under VPN – IPSEC Wizard

Give the VPN a “Name” and template type is “Remote Access”

<img src="media/image30.png"
style="width:6.26806in;height:1.44375in" />

<img src="media/image31.png"
style="width:6.26806in;height:1.68958in" />

<img src="media/image32.png"
style="width:6.26806in;height:2.09375in" />

<img src="media/image33.png"
style="width:6.26806in;height:1.84653in" />

<img src="media/image34.png"
style="width:6.26806in;height:2.98333in" />

<img src="media/image35.png"
style="width:6.26806in;height:5.17431in" />

Click “Convert to Custom tunnel”

<img src="media/image36.png"
style="width:6.26806in;height:2.56319in" />

Change IKE version from 1 to 2

<img src="media/image37.png"
style="width:6.26806in;height:2.31806in" />

Edit Phase 1Proposal

<img src="media/image38.png"
style="width:6.26806in;height:3.48403in" />

<img src="media/image39.png"
style="width:6.26806in;height:2.46736in" />

<img src="media/image40.png"
style="width:3.98863in;height:5.44185in" />

<img src="media/image41.png"
style="width:4.51698in;height:4.90732in" />

Now in the CLI do the below:

<img src="media/image42.png"
style="width:6.26806in;height:0.85139in" />

# VPN Client

Add a new “IPsec VPN” give the connection a name, fill in the remote
gateway with either a FQDN (preferred) or the IP Address, the tick
“Single Sign On Settings” and specify the custom port, 1001 is the
default.

<img src="media/image43.png"
style="width:4.40319in;height:2.37185in" />

<img src="media/image44.png"
style="width:3.25675in;height:0.8295in" />

<img src="media/image45.png"
style="width:3.22097in;height:1.68243in" />

<img src="media/image46.png"
style="width:3.22461in;height:1.44029in" />



