# autofs-smb-gnomekeyring

The scripts in this repository allow you to automatically mount SMB/CIFS share using autofs.  They are derived from the state of the autofs package in debian testing as of 2014-12-29.  Instead of having to store the credentials in a plain text file in /etc/creds/ this configuration allows you to store the credentials in your gnome-keyring.

## Install

These instructions are based on debian, I have not tested this on other distributions.

* Install autofs : sudo apt-get install autofs
* Install gkeyring : see https://github.com/kparal/gkeyring
* Place the file gkeyring-as in /usr/local/bin, ensure it is executable
* Place the file auto.smb.gnomekeyring in /etc/, ensure it is executable
* Adjust /etc/auto.master accordingly, for example add the following line:

```
/mnt/cifs/      /etc/auto.smb.gnomekeyring      --timeout=600,--ghost
```

* Restart the autofs service : sudo service autofs restart

## Getting started

* Ensure credentials for your remote server are stored in gnome-keyring as network credentials.   Each credential should have a server attribute that is set to the name or address of the remote server you want these	credentials to apply to.  The protocol attribute of the credentials should be set to smb.

* cd to the subdirectory of your autofs mounted location (/mnt/cifs in the example above) that has the same name as your server (or address).  The subdirectory may not yet exist.

## Example

```
gkeyring -s --name='johndoe@fileserver' --type=network -p"server=fileserver,protocol=smb,domain=NETWORK,user=johndoe"
... Enter password
cd /mnt/cifs/fileserver
```