# Pi.Alert install on Debian Instructions

The following are notes for installation in my environment.

### Proxmox vm configuarion information

* Using: `debian-12.5.0-amd64-netinst.iso` on Proxmox server
* VM default: 2g ram, 1 proc, 32gig disk

### Debian installation configuarion information

* Hostname: depi
* Secondary user: pi/pi
* Only choose ssh server and standard system utilities
* *Otherwise defaults*

### Pi.Alert

1. Login to server
    ```bash
    ssh pi@depi
    ```
2. Install curl
    ```bash
    su -
    apt install wget curl vim sudo -y
    usermod -aG sudo pi
    exit;exit
    ```

4. Install pi.alert
    ```bash
    ssh pi@depi
    wget https://github.com/pucherot/Pi.Alert/raw/main/install/pialert_install.sh
    bash pialert_install.sh
    ```
   * Choose options
     1. **path:** /root/pialert
     2. **Install Pi-hole** yes
     3. **DHCP** No
     4. **Python** 3
     5. **New Devices as known** yes
     6. **e-mail alert** no
     7. **dynu.net** no
     8. **Pi-hole** continue on static ip (ignore)
     9. **Upstream DNS** Quad9 (filtered, ECS, DNSSEC)
     10. **Rest of Pi hole** Yes,Yes,Yes
     11. Write down Pi hole admin password

# Active URLS

* http://192.168.x.x/pialert
* http://192.168.x.x/admin

# Fixes
  ```bash
  sudo pihole -a -p pi
  mv /var/www/html/index.lighttpd.html /var/www/html/index.lighttpd.oem
  echo 'server.dir-listing = "enable"' | sudo tee -a /etc/lighttpd/lighttpd.conf
  sudo systemctl restart lighttpd.service
  ```

# Installation Log
```log
pi@depi:~$ bash pialert_install.sh

############################################################
 Pi.Alert Installation
############################################################
Sun Apr 28 01:20:30 PM CDT 2024
Logfile: pialert_install_2024-04-28_13-20.log

------------------------------------------------------------
 Pi-hole
------------------------------------------------------------
- Checking if Pi-hole is installed...
- Installing Pi-hole...
  - Pi-hole has its own logfile

  [i] Root user check
  [i] Script called with non-root privileges
      The Pi-hole requires elevated privileges to install and run
      Please check the installer for any concerns regarding this requirement
      Make sure to download this script from a trusted source

  [✓] Sudo utility check

  [✓] Root user check

        .;;,.
        .ccccc:,.
         :cccclll:.      ..,,
          :ccccclll.   ;ooodc
           'ccll:;ll .oooodc
             .;cll.;;looo:.
                 .. ','.
                .',,,,,,'.
              .',,,,,,,,,,.
            .',,,,,,,,,,,,....
          ....''',,,,,,,'.......
        .........  ....  .........
        ..........      ..........
        ..........      ..........
        .........  ....  .........
          ........,,,,,,,'......
            ....',,,,,,,,,,,,.
               .',,,,,,,,,'.
                .',,,,,,'.
                  ..'''.

  [i] SELinux not detected
  [✓] Update local cache of available packages

  [✓] Checking apt-get for upgraded packages... up to date!

  [i] Checking for / installing Required dependencies for OS Check...
  [✓] Checking for grep
  [i] Checking for dnsutils (will be installed)
  [i] Waiting for package manager to finish (up to 30 seconds)
  [i] Processing apt-get install(s) for: dnsutils, please wait...
----------------------------------------------------------------------
Selecting previously unselected package dnsutils.
(Reading database ... 33466 files and directories currently installed.)
Preparing to unpack .../dnsutils_1%3a9.18.24-1_all.deb ...
Unpacking dnsutils (1:9.18.24-1) ...
Setting up dnsutils (1:9.18.24-1) ...
----------------------------------------------------------------------
  [✓] Supported OS detected
  [i] Checking for / installing Required dependencies for this install script...
  [i] Checking for git (will be installed)
  [✓] Checking for iproute2
  [i] Checking for dialog (will be installed)
  [✓] Checking for ca-certificates
  [i] Waiting for package manager to finish (up to 30 seconds)
  [i] Processing apt-get install(s) for: git dialog, please wait...
----------------------------------------------------------------------
Selecting previously unselected package dialog.
(Reading database ... 33470 files and directories currently installed.)
Preparing to unpack .../dialog_1.3-20230209-1_amd64.deb ...
Unpacking dialog (1.3-20230209-1) ...
Selecting previously unselected package liberror-perl.
Preparing to unpack .../liberror-perl_0.17029-2_all.deb ...
Unpacking liberror-perl (0.17029-2) ...
Selecting previously unselected package git-man.
Preparing to unpack .../git-man_1%3a2.39.2-1.1_all.deb ...
Unpacking git-man (1:2.39.2-1.1) ...
Selecting previously unselected package git.
Preparing to unpack .../git_1%3a2.39.2-1.1_amd64.deb ...
Unpacking git (1:2.39.2-1.1) ...
Setting up liberror-perl (0.17029-2) ...
Setting up dialog (1.3-20230209-1) ...
Setting up git-man (1:2.39.2-1.1) ...
Setting up git (1:2.39.2-1.1) ...
Processing triggers for man-db (2.11.2-2) ...
----------------------------------------------------------------------
  [i] IPv4 address: 192.168.x.x/24
  [i] Unable to find IPv6 ULA/GUA address
  [i] IPv6 address:
  [i] Using upstream DNS: Quad9 (filtered, ECS, DNSSEC) (9.9.9.11, 149.112.112.11)
  [i] Installing StevenBlack's Unified Hosts List
  [i] Installing Admin Web Interface
  [i] Installing lighttpd
  [i] Query Logging on.
  [i] Using privacy level: 0
  [✗] Check for existing repository in /etc/.pihole
  [i] Clone https://github.com/pi-hole/pi-hole.git into /etc/.pihole...HEAD is now at 5490a6e Release 5.18.2 (#5629)
  [✓] Clone https://github.com/pi-hole/pi-hole.git into /etc/.pihole

  [✗] Check for existing repository in /var/www/html/admin
  [i] Clone https://github.com/pi-hole/web.git into /var/www/html/admin...HEAD is now at be05b0f v5.21 (#2860)
  [✓] Clone https://github.com/pi-hole/web.git into /var/www/html/admin

  [i] Checking for / installing Required dependencies for Pi-hole software...
  [✓] Checking for cron
  [✓] Checking for curl
  [✓] Checking for iputils-ping
  [i] Checking for psmisc (will be installed)
  [✓] Checking for sudo
  [i] Checking for unzip (will be installed)
  [i] Checking for idn2 (will be installed)
  [✓] Checking for libcap2-bin
  [i] Checking for dns-root-data (will be installed)
  [✓] Checking for libcap2
  [i] Checking for netcat-openbsd (will be installed)
  [✓] Checking for procps
  [i] Checking for jq (will be installed)
  [i] Checking for lighttpd (will be installed)
  [i] Checking for php-common (will be installed)
  [i] Checking for php-cgi (will be installed)
  [i] Checking for php-sqlite3 (will be installed)
  [i] Checking for php-xml (will be installed)
  [i] Checking for php-intl (will be installed)
  [i] Checking for php-json (will be installed)
  [i] Waiting for package manager to finish (up to 30 seconds)
  [i] Processing apt-get install(s) for: psmisc unzip idn2 dns-root-data netcat-openbsd jq lighttpd php-common php-cgi php-sqlite3 php-xml php-intl php-json, please wait...
----------------------------------------------------------------------
Selecting previously unselected package lighttpd.
(Reading database ... 34723 files and directories currently installed.)
Preparing to unpack .../00-lighttpd_1.4.69-1_amd64.deb ...
Unpacking lighttpd (1.4.69-1) ...
Selecting previously unselected package dns-root-data.
Preparing to unpack .../01-dns-root-data_2023010101_all.deb ...
Unpacking dns-root-data (2023010101) ...
Selecting previously unselected package idn2.
Preparing to unpack .../02-idn2_2.3.3-1+b1_amd64.deb ...
Unpacking idn2 (2.3.3-1+b1) ...
Selecting previously unselected package libonig5:amd64.
Preparing to unpack .../03-libonig5_6.9.8-1_amd64.deb ...
Unpacking libonig5:amd64 (6.9.8-1) ...
Selecting previously unselected package libjq1:amd64.
Preparing to unpack .../04-libjq1_1.6-2.1_amd64.deb ...
Unpacking libjq1:amd64 (1.6-2.1) ...
Selecting previously unselected package jq.
Preparing to unpack .../05-jq_1.6-2.1_amd64.deb ...
Unpacking jq (1.6-2.1) ...
Selecting previously unselected package libsodium23:amd64.
Preparing to unpack .../06-libsodium23_1.0.18-1_amd64.deb ...
Unpacking libsodium23:amd64 (1.0.18-1) ...
Selecting previously unselected package libxslt1.1:amd64.
Preparing to unpack .../07-libxslt1.1_1.1.35-1_amd64.deb ...
Unpacking libxslt1.1:amd64 (1.1.35-1) ...
Selecting previously unselected package netcat-openbsd.
Preparing to unpack .../08-netcat-openbsd_1.219-1_amd64.deb ...
Unpacking netcat-openbsd (1.219-1) ...
Selecting previously unselected package psmisc.
Preparing to unpack .../09-psmisc_23.6-1_amd64.deb ...
Unpacking psmisc (23.6-1) ...
Selecting previously unselected package php-common.
Preparing to unpack .../10-php-common_2%3a93_all.deb ...
Unpacking php-common (2:93) ...
Selecting previously unselected package php8.2-common.
Preparing to unpack .../11-php8.2-common_8.2.18-1~deb12u1_amd64.deb ...
Unpacking php8.2-common (8.2.18-1~deb12u1) ...
Selecting previously unselected package php8.2-opcache.
Preparing to unpack .../12-php8.2-opcache_8.2.18-1~deb12u1_amd64.deb ...
Unpacking php8.2-opcache (8.2.18-1~deb12u1) ...
Selecting previously unselected package php8.2-readline.
Preparing to unpack .../13-php8.2-readline_8.2.18-1~deb12u1_amd64.deb ...
Unpacking php8.2-readline (8.2.18-1~deb12u1) ...
Selecting previously unselected package php8.2-cli.
Preparing to unpack .../14-php8.2-cli_8.2.18-1~deb12u1_amd64.deb ...
Unpacking php8.2-cli (8.2.18-1~deb12u1) ...
Selecting previously unselected package php8.2-cgi.
Preparing to unpack .../15-php8.2-cgi_8.2.18-1~deb12u1_amd64.deb ...
Unpacking php8.2-cgi (8.2.18-1~deb12u1) ...
Selecting previously unselected package php-cgi.
Preparing to unpack .../16-php-cgi_2%3a8.2+93_all.deb ...
Unpacking php-cgi (2:8.2+93) ...
Selecting previously unselected package php8.2-intl.
Preparing to unpack .../17-php8.2-intl_8.2.18-1~deb12u1_amd64.deb ...
Unpacking php8.2-intl (8.2.18-1~deb12u1) ...
Selecting previously unselected package php-intl.
Preparing to unpack .../18-php-intl_2%3a8.2+93_all.deb ...
Unpacking php-intl (2:8.2+93) ...
Selecting previously unselected package php-json.
Preparing to unpack .../19-php-json_2%3a8.2+93_all.deb ...
Unpacking php-json (2:8.2+93) ...
Selecting previously unselected package php8.2-sqlite3.
Preparing to unpack .../20-php8.2-sqlite3_8.2.18-1~deb12u1_amd64.deb ...
Unpacking php8.2-sqlite3 (8.2.18-1~deb12u1) ...
Selecting previously unselected package php-sqlite3.
Preparing to unpack .../21-php-sqlite3_2%3a8.2+93_all.deb ...
Unpacking php-sqlite3 (2:8.2+93) ...
Selecting previously unselected package php8.2-xml.
Preparing to unpack .../22-php8.2-xml_8.2.18-1~deb12u1_amd64.deb ...
Unpacking php8.2-xml (8.2.18-1~deb12u1) ...
Selecting previously unselected package php-xml.
Preparing to unpack .../23-php-xml_2%3a8.2+93_all.deb ...
Unpacking php-xml (2:8.2+93) ...
Selecting previously unselected package unzip.
Preparing to unpack .../24-unzip_6.0-28_amd64.deb ...
Unpacking unzip (6.0-28) ...
Setting up lighttpd (1.4.69-1) ...
Enabling unconfigured: ok
Run "service lighttpd force-reload" to enable changes
Created symlink /etc/systemd/system/multi-user.target.wants/lighttpd.service → /lib/systemd/system/lighttpd.service.
Setting up idn2 (2.3.3-1+b1) ...
Setting up libsodium23:amd64 (1.0.18-1) ...
Setting up psmisc (23.6-1) ...
Setting up unzip (6.0-28) ...
Setting up netcat-openbsd (1.219-1) ...
update-alternatives: using /bin/nc.openbsd to provide /bin/nc (nc) in auto mode
Setting up dns-root-data (2023010101) ...
Setting up libxslt1.1:amd64 (1.1.35-1) ...
Setting up libonig5:amd64 (6.9.8-1) ...
Setting up php-common (2:93) ...
Created symlink /etc/systemd/system/timers.target.wants/phpsessionclean.timer → /lib/systemd/system/phpsessionclean.timer.
Setting up php8.2-common (8.2.18-1~deb12u1) ...

Creating config file /etc/php/8.2/mods-available/calendar.ini with new version

Creating config file /etc/php/8.2/mods-available/ctype.ini with new version

Creating config file /etc/php/8.2/mods-available/exif.ini with new version

Creating config file /etc/php/8.2/mods-available/fileinfo.ini with new version

Creating config file /etc/php/8.2/mods-available/ffi.ini with new version

Creating config file /etc/php/8.2/mods-available/ftp.ini with new version

Creating config file /etc/php/8.2/mods-available/gettext.ini with new version

Creating config file /etc/php/8.2/mods-available/iconv.ini with new version

Creating config file /etc/php/8.2/mods-available/pdo.ini with new version

Creating config file /etc/php/8.2/mods-available/phar.ini with new version

Creating config file /etc/php/8.2/mods-available/posix.ini with new version

Creating config file /etc/php/8.2/mods-available/shmop.ini with new version

Creating config file /etc/php/8.2/mods-available/sockets.ini with new version

Creating config file /etc/php/8.2/mods-available/sysvmsg.ini with new version

Creating config file /etc/php/8.2/mods-available/sysvsem.ini with new version

Creating config file /etc/php/8.2/mods-available/sysvshm.ini with new version

Creating config file /etc/php/8.2/mods-available/tokenizer.ini with new version
Setting up libjq1:amd64 (1.6-2.1) ...
Setting up php8.2-opcache (8.2.18-1~deb12u1) ...

Creating config file /etc/php/8.2/mods-available/opcache.ini with new version
Setting up php8.2-readline (8.2.18-1~deb12u1) ...

Creating config file /etc/php/8.2/mods-available/readline.ini with new version
Setting up php8.2-intl (8.2.18-1~deb12u1) ...

Creating config file /etc/php/8.2/mods-available/intl.ini with new version
Setting up php-intl (2:8.2+93) ...
Setting up php8.2-xml (8.2.18-1~deb12u1) ...

Creating config file /etc/php/8.2/mods-available/dom.ini with new version

Creating config file /etc/php/8.2/mods-available/simplexml.ini with new version

Creating config file /etc/php/8.2/mods-available/xml.ini with new version

Creating config file /etc/php/8.2/mods-available/xmlreader.ini with new version

Creating config file /etc/php/8.2/mods-available/xmlwriter.ini with new version

Creating config file /etc/php/8.2/mods-available/xsl.ini with new version
Setting up jq (1.6-2.1) ...
Setting up php8.2-cli (8.2.18-1~deb12u1) ...
update-alternatives: using /usr/bin/php8.2 to provide /usr/bin/php (php) in auto mode
update-alternatives: using /usr/bin/phar8.2 to provide /usr/bin/phar (phar) in auto mode
update-alternatives: using /usr/bin/phar.phar8.2 to provide /usr/bin/phar.phar (phar.phar) in auto mode

Creating config file /etc/php/8.2/cli/php.ini with new version
Setting up php8.2-sqlite3 (8.2.18-1~deb12u1) ...

Creating config file /etc/php/8.2/mods-available/sqlite3.ini with new version

Creating config file /etc/php/8.2/mods-available/pdo_sqlite.ini with new version
Setting up php-xml (2:8.2+93) ...
Setting up php-json (2:8.2+93) ...
Setting up php-sqlite3 (2:8.2+93) ...
Setting up php8.2-cgi (8.2.18-1~deb12u1) ...
update-alternatives: using /usr/bin/php-cgi8.2 to provide /usr/bin/php-cgi (php-cgi) in auto mode
update-alternatives: using /usr/lib/cgi-bin/php8.2 to provide /usr/lib/cgi-bin/php (php-cgi-bin) in auto mode

Creating config file /etc/php/8.2/cgi/php.ini with new version
Setting up php-cgi (2:8.2+93) ...
update-alternatives: using /usr/bin/php-cgi.default to provide /usr/bin/php-cgi (php-cgi) in auto mode
update-alternatives: using /usr/lib/cgi-bin/php.default to provide /usr/lib/cgi-bin/php (php-cgi-bin) in auto mode
Processing triggers for man-db (2.11.2-2) ...
Processing triggers for mailcap (3.70+nmu1) ...
Processing triggers for libc-bin (2.36-9+deb12u6) ...
Processing triggers for php8.2-cli (8.2.18-1~deb12u1) ...
Processing triggers for php8.2-cgi (8.2.18-1~deb12u1) ...
----------------------------------------------------------------------
  [✓] Enabling lighttpd service to start on reboot...
  [✗] Checking for group 'pihole'
  [✓] Creating group 'pihole'
  [✓] Creating user 'pihole'

  [i] FTL Checks...

  [✓] Detected x86_64 processor
  [i] Checking for existing FTL binary...
  [✓] Downloading and Installing FTL
  [✓] Installing scripts from /etc/.pihole

  [i] Installing configs from /etc/.pihole...
  [✓] No dnsmasq.conf found... restoring default dnsmasq.conf...
  [✓] Installed /etc/dnsmasq.d/01-pihole.conf
  [✓] Installed /etc/dnsmasq.d/06-rfc6761.conf

  [✓] Installing sudoer file

  [✓] Installing latest Cron script

  [✓] Installing latest logrotate script
  [i] Backing up /etc/dnsmasq.conf to /etc/dnsmasq.conf.old
  [✓] man pages installed and database updated
  [i] Testing if systemd-resolved is enabled
  [i] Systemd-resolved is not enabled
  [✓] Restarting lighttpd service...
  [✓] Enabling lighttpd service to start on reboot...
  [i] Restarting services...
  [✓] Enabling pihole-FTL service to start on reboot...
  [✓] Restarting pihole-FTL service...
  [i] Creating new gravity database
  [i] Migrating content of /etc/pihole/adlists.list into new database
  [✓] Deleting existing list cache
  [i] Neutrino emissions detected...
  [✓] Pulling blocklist source list into range

  [✓] Preparing new gravity database
  [✓] Creating new gravity databases
  [i] Using libz compression

  [i] Target: https://raw.githubusercontent.com/StevenBlack/hosts/master/hosts
  [✓] Status: Retrieval successful
  [✓] Parsed 128536 exact domains and 0 ABP-style domains (ignored 1 non-domain entries)
      Sample of non-domain entries:
        - "0.0.0.0"


  [✓] Building tree
  [✓] Swapping databases
  [✓] The old database remains available
  [i] Number of gravity domains: 128536 (128536 unique domains)
  [i] Number of exact blacklisted domains: 0
  [i] Number of regex blacklist filters: 0
  [i] Number of exact whitelisted domains: 0
  [i] Number of regex whitelist filters: 0
  [✓] Flushing DNS cache
  [✓] Cleaning up stray matter

  [✓] FTL is listening on port 53
     [✓] UDP (IPv4)
     [✓] TCP (IPv4)
     [✓] UDP (IPv6)
     [✓] TCP (IPv6)

  [i] Pi-hole blocking will be enabled
  [i] Enabling blocking
  [✓] Reloading DNS lists
  [✓] Pi-hole Enabled
  [i] Web Interface password: 1izsgilN
  [i] This can be changed using 'pihole -a -p'

  [i] View the web interface at http://pi.hole/admin or http://192.168.x.x/admin

  [i] You may now configure your devices to use the Pi-hole as their DNS server
  [i] Pi-hole DNS (IPv4): 192.168.x.x
  [i] If you have not done so already, the above IP should be set to static.

  [i] The install log is located at: /etc/pihole/install.log
  [✓] Installation complete!

- Checking if 'pi.alert' is configured in Local DNS...
- Adding 'pi.alert' to Local DNS...

------------------------------------------------------------
 Lighttpd & PHP
------------------------------------------------------------
- Installing apt-utils...
- Installing lighttpd...
- Installing PHP...
- Activating PHP...
- Restarting lighttpd...
- Installing sqlite3...

------------------------------------------------------------
 arp-scan & dnsutils
------------------------------------------------------------
- Installing arp-scan...
Extracting templates from packages: 100%
- Testing arp-scan...
WARNING: Cannot open MAC/Vendor file ieee-oui.txt: Permission denied
WARNING: Cannot open MAC/Vendor file mac-vendor.txt: Permission denied
192.168.x.x     x:x:x:x:x:x       (Unknown)

- Installing dnsutils & net-tools...

------------------------------------------------------------
 Python
------------------------------------------------------------
- Checking Python 2...
  - Python 2 is NOT installed

- Checking Python 3...
  - Python 3 is installed
    - Python 3.11.2

- Using Python 3

------------------------------------------------------------
 Pi.Alert
------------------------------------------------------------
- Downloading installation tar file...
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
100 55.9M  100 55.9M    0     0  29.1M      0  0:00:01  0:00:01 --:--:-- 80.3M

- Uncompressing tar file
.........................................................
- Deleting downloaded tar file...
- Settting Pi.Alert config file
- Testing Pi.Alert HW vendors database update process...
*** PLEASE WAIT A COUPLE OF MINUTES...
/usr/sbin/get-iab is no longer needed and was depreciated in arp-scan 1.10
get-oui is now used to fetch all of the Ethernet MAC-Vendor
mappings from the various IEEE registries.  See get-oui(1) for
details.
Zero-sized response from from file:///var/lib/ieee-data/iab.csv

Pi.Alert 3.02 (2021-04-24)
---------------------------------------------------------
Update HW Vendors
    Timestamp: 2024-04-28 13:27:00

Updating vendors DB (iab & oui)...
Traceback (most recent call last):
  File "/home/pi/pialert/back/pialert.py", line 1488, in <module>
    sys.exit(main())
             ^^^^^^
  File "/home/pi/pialert/back/pialert.py", line 83, in main
    res = update_devices_MAC_vendors('-s')
          ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/pi/pialert/back/pialert.py", line 264, in update_devices_MAC_vendors
    update_output = subprocess.check_output (update_args)
                    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.11/subprocess.py", line 466, in check_output
    return run(*popenargs, stdout=PIPE, timeout=timeout, check=True,
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.11/subprocess.py", line 571, in run
    raise CalledProcessError(retcode, process.args,
subprocess.CalledProcessError: Command '['sh', '/home/pi/pialert/back/update_vendors.sh', '-s']' returned non-zero exit status 255.

- Testing Pi.Alert Internet IP Lookup...

Pi.Alert 3.02 (2021-04-24)
---------------------------------------------------------
Check Internet IP
    Timestamp: 2024-04-28 13:27:00

Retrieving Internet IP...
    Error retrieving Internet IP
    Exiting...


- Testing Pi.Alert Network scan...
*** PLEASE WAIT A COUPLE OF MINUTES...
WARNING: Cannot open MAC/Vendor file ieee-oui.txt: Permission denied
WARNING: Cannot open MAC/Vendor file mac-vendor.txt: Permission denied

Pi.Alert 3.02 (2021-04-24)
---------------------------------------------------------
Scan Devices
    ScanCycle: 1
    Timestamp: 2024-04-28 13:27:00

Scanning...
    arp-scan Method...
    Pi-hole Method...
    DHCP Leases Method...

Processing scan results...
    Devices Detected.......: 20
        arp-scan Method....: 19
        Pi-hole Method.....: +0
        New Devices........: 20

    Devices in this cycle..: 0
        Down Alerts........: 1
        New Down Alerts....: 1
        New Connections....: 0
        Disconnections.....: 1
        IP Changes.........: 0

Updating DB Info...
    Sessions Events (connect / discconnect) ...
    Creating new devices...
    Updating Devices Info...
        Trying to resolve devices without name.......................
        Names updated:   17
    Voiding false (ghost) disconnections...
    Pairing session events (connection / disconnection) ...
    Creating sessions snapshot...
    Skipping repeated notifications...

Reporting...
    Formating report...
    Skip mail...
    Notifications: 21

DONE!!!



- Set devices as Known devices...
- Adding jobs to the crontab...
- Setting permissions...
- Publishing Pi.Alert web...
- Configuring http://pi.alert/ redirection...
- Restarting lighttpd...

------------------------------------------------------------
 Installation process finished
------------------------------------------------------------
Use: - http://pi.alert/
     - http://192.168.x.x/pialert/
To access Pi.Alert web
```


