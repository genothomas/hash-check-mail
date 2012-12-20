# check-file.py - Python script to monitor a file for changes and then mail the report with the file attached.

## Summary
This is a script which checks a file's md5 hash, compares it to a previous (or given) hash and mails a report with the option of attaching the file with the email. I wrote it because I use AIDE on some systems, and I let it auto update the database. This script runs via cron before and after the AIDE run, so I have an archive of databases. But it can be used for all kind of files, not just for the AIDE database.

### Requirements

- Python 2.7 or above (tested in python 2.7.3 and python 3.3.0)

### Download

Clone this git repo:
    
    git clone https://github.com/RaymiiOrg/hash-check-mail.git

Or download it from Raymii.org:

    wget http://raymii.org/s/inc/downloads/check-file.py

### Usage

Usage of the script is fairly simple. I'll explain the command line options below:


- -c --checksum-compare
The MD5SUM you wish to compare the file to. If this option is given, the script will not write the checksum to a temp location for comparison (/tmp). This should be used if the checksum of the file is known and should not change.

- -o --overwrite-checksum
When this option is given, when the checksum of a file has been changed, the script will write the new checksum to the temp file. This way you get an email when a change occurs, but changes are allowed. My AIDE database is known to change, but I still want to know when it changes.

- -e --email 
The email address to send the report to. Defaults to `root@$hostname`.

- -a --attach-file
When this option is given, the file which is checked is sent as an attachment with the email. My AIDE database gets mailed every time it changes. This way you can inspect the changed file without logging on to the remote system, or keep it for archival purposes.

- -s --smtp 
Hostname or IP address of the SMTP server. Defaults to `localhost`.

- -f --sender
The `From` address of the email. Defaults to `root@$hostname`.

- -m --mail-on-match
When this option is given, it mails when the file matches. By default no email is sent when the file checksum matches. With this switch you can force an email when the file checksum has not been changed.

### Examples

1. Monitor the file `/var/lib/aide/aide.db.gz`, mail to `admin@domain.com`, overwrite checksum on change and attach the file:

    ./check-file.py  -e "admin@domain.com" -a -o /var/lib/aide/aide.db.gz

2. Monitor the file `/var/lib/aide/aide.db.gz` against known checksum and mail to `admin@domain.com`:

    ./check-file.py -e "admin@domain.com" -a -c "41ce523dd72a67039df2db9b4542411c" /var/lib/aide/aide.db.gz

3. Monitor file `/etc/passwd`, send even if md5sum matches, from `customer@kpn.nl` using smtp server `smtp.kpnmail.com`, do not attach it to the email and do not overwrite the checksum file:
    
    ./check-file.py -e "admin@domain.com" -f "customer@kpn.nl" -s "smtp.kpnmail.com" -m /etc/passwd

4. Cron job to monitor the AIDE database and mail changes every night at 10 PM:

    1 22 * * * /usr/bin/python /root/scripts/check-file.py -e "admin@domain.com" -a -o /var/lib/aide/aide.db.gz

### Sample report email

    File checksum report (v 0.1).

    Date: Sat, 15 Dec 2012 21:18:26 +0000. 
    Hostname: aide.raymii.org.

    Checksum does NOT match.



    Old: 54b0a4497e2b1d9b8e9dc704838e9047. 
    New: 38293053ec07539050f3efd9d33b310b. 
    Filename: /usr/share/doc/munin-node/README.Debian. 
    I checksummed the file itself.
    Overwriting old checksum with new checksum as requested. 
    Written checksum 38293053ec07539050f3efd9d33b310b to file /tmp/usr-share-doc-munin-node-README.Debian.md5

### More info:

See [https://raymii.org/s/software/Python-script-to-monitor-a-file-for-changes-and-then-mail-the-report-with-the-file-attached.html](https://raymii.org/s/software/Python-script-to-monitor-a-file-for-changes-and-then-mail-the-report-with-the-file-attached.html).

### License

    # Copyright (C) 2012  Remy van Elst

    # This program is free software: you can redistribute it and/or modify
    # it under the terms of the GNU General Public License as published by
    # the Free Software Foundation, either version 3 of the License, or
    # (at your option) any later version.

    # This program is distributed in the hope that it will be useful,
    # but WITHOUT ANY WARRANTY; without even the implied warranty of
    # MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    # GNU General Public License for more details.

    # You should have received a copy of the GNU General Public License
    # along with this program.  If not, see <http://www.gnu.org/licenses/>.
