### Introduction

A FreeRadius Config for WPA2 Enterprise to allow random user/password combinations and put the users into a VLAN.
Bonus: Define Users and give them custom VLANs.

- Tested with:
  - APs 
    - Ubiquiti Unifi AC Series
  - Clients
    - Android Devices
    - Linux (NetworkManager)
    - iOS 11
    - MacOS High Sierra

### Usage

- Install FreeRadius
- Obtain Certificates (Self signed or Let's Encrypt)
- Generate a Diffie-Hellman File
```
# openssl dhparam -out dh.pem -2 2048
```
- Edit the following files according to your needs:
  - clients.conf - Add your IP-Ranges and define a shared secret
  - users - Define your VLANs and Users
  - mods-enabled/eap - Link your TLS Certificate and Diffie-Hellman file
- Connect your devices \o/

### Connecting

#### Android
EAP method: TTLS
Phase-2 Authentification: PAP
CA-Certificate: Use system certificates
Domain: [your letsencrypt domain]
Identity: [your user]
Password: [your password]

#### NetworkManager
Example Config:
```
# /etc/NetworkManager/system-connections/ExampleWifi
[connection]
id=ExampleWifi # CHANGE THIS
uuid=a64f1a2f-39c2-46b4-9051-5b390d07b97e
type=wifi
permissions=

[wifi]
mac-address=00:00:00:00:13:37 ## CHANGE THIS
mac-address-blacklist=
mode=infrastructure
ssid=ExampleWifi ## CHANGE THIS

[wifi-security]
auth-alg=open
group=
key-mgmt=wpa-eap
pairwise=
proto=

[802-1x]
altsubject-matches=DNS:your.domain.tld ## CHANGE THIS
ca-cert=/etc/ssl/certs/DST_Root_CA_X3.pem
eap=ttls;
identity=user ## CHANGE THIS
password=password ## CHANGE THIS
phase2-altsubject-matches=
phase2-auth=pap

[ipv4]
dns-search=
method=auto

[ipv6]
addr-gen-mode=stable-privacy
dns-search=
method=auto                       
```

