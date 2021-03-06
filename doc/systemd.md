SYSTEMD SUPPORT IN BARCOIN
===========================

Packagers can find a .service file in this repo in order to integrate barcoin's 
daemon into systemd based distributions.

barcoin.service file is located in contrib/systemd/ folder.

1. Users
---------------------------------

This .service file assumes barcoind user and group exist in the system, so packager
should make sure they are created on installation. 

2. Files
---------------------------------

The .service file assumes several paths that might need to be adjusted according
to packager's needs.

Daemon's config file is assumed to be located at /etc/barcoind.conf (you can
use contrib/debian/examples/barcoin.conf as an example). Once installed, users
must edit the file in order to update at least these two 
values: rpcuser and rpcpassword . Failing to do so will make the daemon fail 
to boot. However, the message written to /var/lib/barcoind/debug.log file is
very helpful and no default values should be set:

    YYYY-MM-DD HH:MM:DD Error: To use the "-daemon" option, you must set a rpcpassword in the configuration file:
    /etc/barcoind.conf
    It is recommended you use the following random password:
    rpcuser=barcoinrpc
    rpcpassword=6q1EG75KyvoFXjgfUPXtEKqfDKJJyyXaMSMsBY21jAUb
    (you do not need to remember this password)
    The username and password MUST NOT be the same.
    If the file does not exist, create it with owner-readable-only file permissions.
    It is also recommended to set alertnotify so you are notified of problems;
    for example: alertnotify=echo %s | mail -s "Barcoin Alert" admin@foo.com


Daemon's data and pid files will be stored in /var/lib/barcoind directory, so it
should be created on installation and make barcoind user/group it's owner.

3. Installing .service file
---------------------------------

Installing this .service file consists on just copying it to /usr/lib/systemd/system
directory, followed by the command "systemctl daemon-reload" in order to update
running systemd configuration.