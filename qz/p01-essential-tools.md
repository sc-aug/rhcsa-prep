UNDERSTAND AND USE ESSENTIAL TOOLS

1) How would you dump the contents of the /var/log/messages file into standard output, grep for all lines that contain "Memory" and then redirect the grep'ed result to /home/user/log.txt?
* cat /var/log/messages | grep -i memory > /home/user/log.txt

2) How would you append the text "service=on" to the /etc/motd file?
* echo "service=on" >> /etc/motd

3) Chown apache:apache -R /var/www recursively sets the owner to all files and directories under /var/www/ to apache and the group ownership to apache.
* True

4) You need to search for man pages that relate to the postfix service. Which command will do this?
* whatis postfix, apropos postfix

5) What command would you use to install the star utility?
* yum install star

6) Which directory contains the info files from which the info program reads?
* /usr/share/info

7) You need to set the execute bit on the finance directory and any subdirectories it might contain, but not on the files within them. This change must apply to all users. What methods might you use?
* chmod a+X -R finance, chmod -R a+X finance