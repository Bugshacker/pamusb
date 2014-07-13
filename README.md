#PAM USB OTP Authentication
-----------

####Ubuntu => /etc/pam.d/common-auth 

####Fedora/RHEL/CentOS => /etc/pam.d/system-auth 

* Install dependencies Ubuntu/Debian

```
sudo apt-get install libpam-usb pamusb-tools
```

* Install dependencies Fedora/RHEL/CentOS/Gentoo

```
sudo yum -y install libxml2 pam udisks pmount
```

* For Fedora/RHEL/CentOS

```
git clone https://github.com/aluzzardi/pam_usb.git
cd pam_usb
make 
make install
```

* Gentoo Instructions [Gentoo pam_usb](https://forums.gentoo.org/viewtopic-t-305540-start-0.html)

```
emerge pam_usb
```


* Configure USB Device 
* 
```
pamusb-conf --add-device $MY_DEVICE_ALIAS
```

* Configure user accounts to authenticate with USB 

```
pamusb-conf --add-user root
pamusb-conf --add-user $some_other_user_name
```

* Check user account configurations 

```
pamusb-check root
```

* Setup PAM System Authentication process Ubuntu EDIT:  **/etc/pam.d/common-auth** RHEL/CentOS/Fedora EDIT: **/etc/pam.d/system-auth** 
  The file should look like this post configuration changing only these lines

```
auth    sufficient      pam_usb.so
#auth   [success=1 default=ignore]      pam_unix.so nullok_secure try_first_pass
auth    required                        pam_unix.so nullok_secure
```

* Setup screen saver lock upon USB device removal EDIT: **/etc/pamusb.conf** - example syntax format for the **root** user account 

```
</user><user id="root">
        <device>falcon-usb-stick</device>
        <agent event="lock">gnome-screensaver-command --lock</agent>
        <agent event="unlock">gnome-screensaver-command --deactivate</agent>
</user></users>
```

* Setup *su* & *sudo* services to require USB one time password authentication 

```
<service id="su">
                        <device>my-usb-stick</device>
                        <option name="enable">true<option>
                        <option name="quiet">true</option>
                </service>
                <service id="sudo">
                        <device>my-usb-stick</device>
                        <option name="enable">true<option>
                        <option name="quiet">true</option>
                </service>
        </services>
```

* Configure the pamusb-agent to startup at Login as a daemon process - under Ubuntu 

####SEARCH -> Startup Applications -> Add

```
/usr/bin/pamusb-agent --daemon
```

* Logout & Login - You have now successfully configured utilizing USB removable device as a One Time Password Authentication Mechanism 




