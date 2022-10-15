# Creating a system user

The following recipe creates a system user. System users are typically unprivileged users who do not have login rights. Running a background unattended service with such a user mitigates several security risks and it is always considered a good practice.

    $ sudo useradd -r <user>
    $ sudo usermod -s /bin/false <user>

The first command creates a user without a home directory or password. The second removes the shell from the user.
 
 In case you need a home as well try the following:
 
    $ sudo useradd -r kodi -m -d /var/<user>