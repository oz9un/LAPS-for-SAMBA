
# LAPS Initialization for SAMBA DC

This repository includes scripts that should be executed on SAMBA DC machine in order to implement LAPS (Local Administrator Password Solution).

For more information about LAPS, you can visit [Microsoft](https://www.microsoft.com/en-us/download/details.aspx?id=46899)'s page.


## What are these scripts doing❓

Implementation process of LAPS on SAMBA DC mainly consists of 3 scripts (*respectively*):

### laps-install:
This script makes necessary schema changes on Samba DC, therefore it should be run first.
- Take DC name as an argument.
- Creates two ldif files according to argument provided by the user.
- Executes ldif files with *ldbadd* and *ldbmodify* commands.

### laps-init:
In order to make LAPS work on Samba DC, following properties should have a initial values:
- **ms-Mcs-AdmPwd**
- **ms-Mcs-AdmPwdExpirationTime**

This script generates a random password for 'ms-Mcs-AdmPwd' field and sets 'ms-Mcs-AdmPwdExpirationTime' to UNIX's release date.

This script takes **DN** as an argument.


### laps-read:
By default, Samba DC is not able to read 'ms-Mcs-AdmPwdExpirationTime' of clients.
This problem can be fixed with basic Powershell commands on Windows side.

This scripts solves that problem on Linux Samba side. It basically edits the '**nTSecurityDescriptor**' property in order to set the necessary permissions.

This script takes **DN** as an argument.

## Installation

Firstly, the **lapsamba** .deb package have to be installed. You can easily download the latest version from the **Releases** on this GitHub repository.

You can install that package in two ways.

Via dpkg:
```bash
dpkg -i lapsamba-****-***.deb
```

![lapsamba_installation](https://user-images.githubusercontent.com/57866851/143541969-148dedf0-92ff-4926-a000-710ba24f123b.png)


Via apt:
```bash
apt install ./lapsamba-****-***.deb
```

![lapsamba_installation2](https://user-images.githubusercontent.com/57866851/143542030-689d95f4-974f-4357-8cb8-0ca66bb9e51d.png)

⚠ *Caution*: These script depends on **ldbadd** and **ldbmodify** tools which are part of the **ldb-tools**.
 **ldb-tools** set as a recommendation on deb package. 
 You do not have to install this dependency if you have packages like **sambahvl** installed, which has already **ldb-tools** inside that.

---

**Run the laps-install**:
```bash
sudo laps-install <dc>
```
![lapsamba_script1](https://user-images.githubusercontent.com/57866851/143542104-190f7305-9b21-416e-897e-81fa737dfd09.png)

---
**Run the laps-init**:
```bash
sudo laps-init <dn>
```
![lapsamba_script2](https://user-images.githubusercontent.com/57866851/143542294-2ebb8c99-1fa0-4747-962d-8a08817555e4.png)

---
**Run the laps-read**:
```bash
sudo laps-read <dn>
```
![lapsamba_script3](https://user-images.githubusercontent.com/57866851/143542440-daf2b3f2-f094-49da-be27-f1ece3c324cd.png)
