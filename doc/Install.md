# Installation

## Basic system

* Create a domain and an email address for the timestamper
* Install the necessary third-party software
```sh
sudo apt install git python3-pygit2 python3-gnupg python3-configargparse
sudo apt install python3-nose # Only needed for testing
```
* Install `zeitgitter` client and server:
```sh
sudo pip install git-timestamp
sudo make install
```
* As user `zeitgitter`, create an OpenPGP key using
  `sudo -H -u zeitgitter gpg --expert --full-gen-key`:
  - All it ever needs to do is signing; encryption is not needed.
  - Choose a long key, so it will last many years.
  - See […/doc/Cryptography.md](../doc/Cryptography.md) for more information.
  - Make sure you minimize the chances for the key to ever leak. **Revocation
    of the key should be avoided,** as this creates an unefined state for the
    key for its entire lifetime, not just only after the revocation. Prefer
    to destroy the key before it falls into wrong hands.
* Enter the maintainer information in the `[Maintainer]` part of
  `/etc/zeitgitter.conf`.
* Chose a unique time interval and offset within that interval to commit your
  changes and cross-timestamp (parameters `commit-interval` and `commit-offset`).
* Configure the remaining parameters, including whether to have upstream
  cross-timestamping.
* Set up a front-end webserver doing HTTPS and proxying.
* Test it thoroughly.
* If your server should be public, create a pull request with your addition to
  `…/doc/ServerList.md`.

## Additionally, for use with the PGP Digital Timestamper

* Install GnuPG 1.x (for downward compatibility with the old PGP 2.x key)  
```sh
sudo apt install gnupg1
```

* Import the PGP Digital Timestamping Service's keys  
```sh
wget -O - http://www.itconsult.co.uk/stamper/stampinf.htm | gpg1 --pgp2 --import
```

* Create a mail account and enter its parameters into the configuration file
  (`email-addres`, `imap-server`, `smtp-server`, `mail-username`,
  and `mail-password`).  
  You may want to use a non-public email address for this; it will not show
  up anywhere and only is used when contacting stamper. You should not use it
  for anything else.
