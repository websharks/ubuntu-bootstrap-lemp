## VirtualBox + Vagrant + Landrush; running Ubuntu 16.04 LTS (Xenial Xersus) w/ Nginx, MariaDB (MySQL), and PHP 7.0

LEMP stack built from the [WebSharks Ubuntu Bootstrap project](https://github.com/websharks/ubuntu-bootstrap).

_Prefer Apache? See also: [websharks/ubuntu-bootstrap-lamp](https://github.com/websharks/ubuntu-bootstrap-lamp)_

![](http://cdn.websharks-inc.com/jaswsinc/uploads/2015/03/os-x-vagrant-virtualbox.png)

### Installation Instructions

#### Step 1: Satisfy Software Requirements

You need to have VirtualBox, Vagrant, and one of two DNS plugin options.

```bash
$ brew cask install virtualbox;
$ brew cask install vagrant;

# Also tweak VirtualBox by running this line please:
# See: <https://github.com/mitchellh/vagrant/issues/3083> for details.
$ VBoxManage dhcpserver remove --netname HostInterfaceNetworking-vboxnet0;

# ---------------------------------------------

# You need only one of these. Please choose:
$ vagrant plugin install vagrant-hostsupdater; # Easiest (recommended).
$ vagrant plugin install landrush; # More difficult, but greater flexibility.

# ---------------------------------------------

$ vagrant plugin install vagrant-triggers; # Optional (recommended).
# This allows for special event handling. Helpful, but not required at this time.
```

_**Note:** If you don't install Landrush and instead you go with the simpler DNS plugin `vagrant-hostsupdater` (or you choose not to install a DNS plugin at all), it will mean that your VM will have a static IP address of: `192.168.62.62`_

_This also means that you can only run a single VM at one time, because the static IP is the same for each VM. If you go this route and also want to run multiple VM instances at the same time you will need to change the IP address in the [`Vagrantfile`](Vagrantfile) for each additional VM that you bring up._

_You can just edit the [`Vagrantfile`](Vagrantfile) and bump the IP from `192.168.62.62` (default),  to `192.168.62.63`, `192.168.62.64`, etc (for each of your additional VMS)._

#### Step 2: Clone GitHub Repo (Ubuntu Bootstrap)

```bash
$ mkdir ~/vms && cd ~/vms;
$ git clone https://github.com/websharks/ubuntu-bootstrap-lemp my.vm;
```

_Note that `my.vm` becomes your domain name. Change it if you like, but it must end with `.my.vm` please._

#### Step 3: Vagrant Up!

```bash
$ cd ~/vms/my.vm;
$ vagrant up;
```

#### Step 4: Confirm it is Working!

Visit: http://my.vm/ or https://my.vm/ ~ You should see a 'Coming Soon' message.

#### Step 5: Install Root CA

If you'd like to always see a green SSL status for your local test sites (i.e., avoid `https://` warnings on anything ending with `.vm`), you can download and install [this root CA certificate file](https://github.com/websharks/ubuntu-bootstrap/blob/master/src/ssl/vm-ca-crt.pem) and set your trust settings to "Always Trust" for this certificate.

Any SSL certificates created by the Ubuntu Bootstrap will use that root CA certificate. Trusting the root CA (it's fake, and only for the Ubuntu Bootstrap project), will green-light all of your local `.vm` domains when accessing them over `https://`. On a Mac, you can simply download, then drag n' drop the certificate file onto your Keychain.app. Open up the settings for that certification in Keychain.app and choose "Always Trust" at the top. Done! :-)

**↑ UPDATE (WARNING):** If you're on a Mac, there is a nasty bug in the Keychain application that can lock your system when attempting to 'Always Trust'. Until that bug is fixed in the Mac OS, please see the command-line alternatives demonstrated [here](https://github.com/websharks/ubuntu-bootstrap/issues/11#issuecomment-224305268) by @jaswsinc and @raamdev. I suggest using [the example given by @raamdev](https://github.com/websharks/ubuntu-bootstrap/issues/11#issuecomment-224332504).

#### Step 6: Add Files to: `~/vms/my.vm/app/src/`

The is the web root and it is shared with the VM. So you can fill this directory with files (from your own local system) and those will be accessible from your browser by visiting: https://my.vm/

##### Built-In WordPress Installer

If you develop WordPress themes/plugins, you might find it helpful to use the built-in WordPress installer. Just run the following commands and then visit https://my.vm/ to complete the installation of WordPress from your browser like always.

```bash
$ cd ~/vms/my.vm;
$ vagrant ssh;
$ sudo /bootstrap/src/wordpress/install;

# This line is optional.
# See: <https://github.com/websharks/ubuntu-bootstrap#testing-wordpress-themesplugins-easily>
# $ sudo /bootstrap/src/wordpress/install-symlinks;
```

#### Step 7: Learn to Use Bundled Tools

A username/password is required to access each of these tools. It is always the same thing.

- Username: `admin` Password: `admin`

Available Tools (Using Any of These is Optional):

- <https://my.vm/---tools/pma/> PhpMyAdmin
  DB name: `db0`, DB username: `admin`, DB password: `admin`
- <https://my.vm/---tools/opcache.php> PHP OPcache extension status dump.
- <https://my.vm/---tools/info.php> `phpinfo()` output page.
- <https://my.vm/---tools/fpm-status.php> PHP-FPM status page.
- <https://my.vm/---tools/status.nginx> NGINX status page (if Nginx was installed).
- <http://my.vm:8025> MailHog web interface for reviewing test emails on a VM.
