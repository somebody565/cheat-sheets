## 1. Installing Samba

To install Samba, we run:

```
sudo apt update
sudo apt install samba
```

We can check if the installation was successful by running:

```
whereis samba
```

The following should be its output:

```
samba: /usr/sbin/samba /usr/lib/samba /etc/samba /usr/share/samba /usr/share/man/man7/samba.7.g
```

## 2. Setting up Samba

Now that Samba is installed, we need to create a directory for it to share:

```
mkdir /home/<username>/sambashare/
```

The command above creates a new folder `sambashare` in our home directory which we will share later.

The configuration file for Samba is located at `/etc/samba/smb.conf`. To add the new directory as a share, we edit the file by running:

```
sudo nano /etc/samba/smb.conf
```

At the bottom of the file, add the following lines:

```
[sambashare]
    comment = Samba on Ubuntu
    path = /home/username/sambashare
    read only = no
    browsable = yes
```

Then press `Ctrl-O` to save and `Ctrl-X` to exit from the _nano_ text editor.

### What we’ve just added

- - comment: A brief description of the share.
- path: The directory of our share.
    
- read only: Permission to modify the contents of the share folder is only granted when the value of this directive is `no`.
    
- browsable: When set to `yes`, file managers such as Ubuntu’s default file manager will list this share under “Network” (it could also appear as browseable).
    

Now that we have our new share configured, save it and restart Samba for it to take effect:

```
sudo service smbd restart
```

Update the firewall rules to allow Samba traffic:

```
sudo ufw allow samba
```

## 3. Setting up User Accounts and Connecting to Share

Since Samba doesn’t use the system account password, we need to set up a Samba password for our user account:

```
sudo smbpasswd -a username
```

**Note**  
Username used must belong to a system account, else it won’t save.

### Connecting to Share

On Ubuntu: Open up the default file manager and click _Connect to Server_ then enter: ![ubuntuctn](https://discourse.ubuntu.com//ubuntucommunity.s3.dualstack.us-east-2.amazonaws.com/original/2X/7/7e0831db9f6dc65fa530832163f6e865d746ea32.png)

On macOS: In the Finder menu, click _Go > Connect to Server_ then enter: ![macosctn](https://discourse.ubuntu.com//ubuntucommunity.s3.dualstack.us-east-2.amazonaws.com/original/2X/0/027b1f7d34951cada9c002c3250c1aff148ccc7b.png)

On Windows, open up File Manager and edit the file path to:

```
\\ip-address\sambashare
```

Note: `ip-address` is the Samba server IP address and `sambashare` is the name of the share.