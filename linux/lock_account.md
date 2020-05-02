# Locking accounts

To lock a Linux account the most obvious way is to remove the password by adding `!` in the password field of `/etc/passwd` for the user of interest using any of the following:

    $ sudo usermod -L user
    $ sudo passwd -l user

For extra safety you might want to add as shell for these users `/bin/false/` or `/bin/nologin`. Nevertheless, if the system is configured to accept passwordless logins the above is not sufficient. You will have to actually expire the account using `chage` (to reenable use `E-1` instead of `E0`).

    $ sudo chage -E0 user
