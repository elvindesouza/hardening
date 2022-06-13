# Hardening

## Enumerating Devices on the local network-

Running either a network scan or by simply logging into the router's internal webserver, we can see a list of hostnames, MAC addresses and IP addresses of devices on the network.

First, change the login to the router administration portal(I _know_ you haven't changed it...)

Explicitly assign IP addresses to all known devices(by MAC), one by one. Use the notes field if available to give a short description for each device. Disable guest networks, and set up your DHCP/whitelist to deny any other devices from joining the network.
From now on, assume all devices on the local network are trusted by default.

## Setting up a File Server

Since the file share is only intended to be used over the local network, plain FTP is being used, as opposed to FTP over TLS or SSH-FTP(SFTP)
Start the FTP server process with `ftpd [options]`. It uses TCP on the port you mention. Along with using a secure FTP implementation(FTPS,SFTP), I highly recommend changing away from the default ports that your server uses. You will also want to disable anonymous and root login.

Edit the /etc/ftpusers file to your liking, and your FTP server should be set up. Connect to this server with an appropriate client, or use functionality from file managers like Thunar, Dolphin, Nautilus, etc. to add the FTP share as a "folder"

## Setting up remote(local) administration

### keygen,agent
If you aim for remote administration, **change your default port**! Also, install an intrusion prevention service like `Fail2ban`. Enable the corresponding sshd service unit file to automatically start the daemon upon startup.

Edit the configuration file at /etc/ssh/sshd_config to change port number, SFTP settings, etc.

<sub>Expose your services to the internet after detective, preventive, and deterrent measures are put in place by setting up port-forwarding on your router.</sub>

## Firewall

Upon setting up these services intended to be used on the local network only, remember to set rules on your gateway/router(internal webserver will have a page for firewall rules), server(here, the system running these SSH/FTP/VNC etc. daemons), as well as client(Microsoft Windows or Linux desktop PCs and laptop devices)

An additional step would be installing your preferred router firmware like `OpenWRT`.

For server or server-like systems running Unix-like OSes, iptables,nftables, or ufw are the first things you should look to. You should also install other IDS/IPS services that specializes in securing each target(be it your FTP,SSH,VNC,git,NFS server)

Purge ridiculous "web search" apps from your system. These are likely adware+spyware. Just use your web browser and a bookmarked search engine

### For client systems-

**Android**-Not typically the target of these attacks, so less security is acceptable

**Microsoft Windows**-Use a ""trusted"" antivirus solution, these typically have PUA,botnet detection and firewall services built-in with reasonable defaults

**Linux**-Nowhere as prevalent as Windows, but you might have to be more careful with linux systems, as attacks crafted for linux machines are usually intended for small to large server farms. Most commonly-botnet processes

Firewall utilities are already mentioned, you can combine this with periodic scans with ClamAV, along with keeping your system up to date.

## Least Privilege

Disable the root account on every system you can get your hands on. Instilling minimal permissions on regular user accounts can lessen the scope of an attack from threats outside your home network.

> Make sure `sudo` is set up before disabling the root account!

> Make sure you edit the `sudo` configuration with `$ visudo`!

Set up your filesystem with appropriate rwx permissions

## Removing unnecessary applications and services

`systemctl` is your Bible here. Systemd manpages are THE FM to R. Disable unnecessary startup services, and disable .services in favour of .sockets. Learning some basic systemd administration is essential in managing linux boxes.

Uninstall software that litters trackers all over your system in favour of FOSS software. Remove VPN "apps", replace them with OpenVPN+configuration files. Remove WhatsApp, Telegram, etc. proprietary "apps" and replace them with the web session instead. Or even better, use FOSS alternatives.

Remove google chrome, install librewolf. Install uBlock Origin [Hard Mode, Do You Dare?](https://github.com/gorhill/uBlock/wiki/Blocking-mode:-hard-mode)

Start off with a user.js from [Arkenfox](https://github.com/arkenfox/user.js/) or [this one if you like to live dangerously](https://github.com/pyllyukko/user.js/)

## Logging
https://wiki.archlinux.org/title/Category:Monitoring
### https://wiki.archlinux.org/title/Category:Logging
Set up syslog-ng or rsyslog.
Also, learn basic systemd/journald usage, and cleaning-rotating operations. Experiment with viewing logs for different boots, looking at boot messages, application logs, logs for specific units

It is easy to script graphical popups/alerts on an event.

> There is no point collecting logs if you're never going to read them

## Encryption

SED
OPAL
data in use
data in motion (HTTPS only)
full system encryption
DNSSEC
gpg with commands
VNC x11vnc, tigervnc
OCSP
snort IPS
doker/flatpak/snap/appimage
VPN
NTPsec

### Partition Encryption-

Use LUKS(LVM additionally). Very easy to use, decrypting to non-technical users can just seem like "logging in to the computer"

### File Encryption-

**gnupg**. Ideal for encrypting important documents, as these files are portable, and have the same name as the original unencrypted file.

### Directory/Container Encryption-

**gocryptfs, eCryptfs or fscrypt**. Since they work on stacked filesystems(FUSE), this will make it harder for non-technical users to unlock and navigate.
Ideal for encrypting archives of documents and downloaded files.

### Password,Sensitive Information Management-

**KeePassXC**. Portable,cross-platform,secure. You can keep this password database file along with the encrypted files and containers synced across systems by running a `Syncthing` server.

> Consider the very real possibility of concurrent access corruption

## Physical Security

Use full disk encryption if you have valuable or sensitive information. Any devices taken out of the house must be visited individually and checked to see if any banking information, credit card numbers, etc. are unsecured.

> Remember, your stolen 10k phone can have enough photographs of you, your signature, and your banking habits/information to successfully impersonate you.

Ensure any mobile payment apps have an "app-lock" on them at the very least. Enable `remote wipe`, and `FindMyPhone`

Make sure your network and electrical wiring is not exposed outside the house, juicy target for rodents and neighbourhood kids.
