# Rclone

The Swiss army knife of cloud storage. Rclone is a command-line program to manage files on cloud storage. It is a feature-rich alternative to cloud vendors’ web storage interfaces. Over 70 cloud storage products support Rclone including S3 object stores, business & consumer file storage services, as well as standard transfer protocols.

The full documentation of Rclone can be found [here](https://rclone.org/docs/).

## Encrypt configuration
To keep all your information safe, you should encrypt your rclone configuration. From there on, you will have to use a password to use rclone, so be sure to store your password in a safe place!

``` hl_lines"5 10 12 14 20"
No remotes found, make a new one?
n) New remote
s) Set configuration password
q) Quit config
n/s/q> s
Your configuration is not encrypted.
If you add a password, you will protect your login information to cloud services.
a) Add Password
q) Quit to main menu
a/q> a
Enter NEW configuration password:
password: <your password won’t show when entered>
Confirm NEW configuration password:
password: <your password won’t show when entered>
Password set
Your configuration is encrypted.
c) Change Password
u) Unencrypt configuration
q) Quit to main menu
c/u/q> q
```

## Setup for Teams storage

## Move data from and to storage

## Backing up data

