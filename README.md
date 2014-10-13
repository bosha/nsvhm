Nginx simple virtual host manager
=====

## About ##

It's a pretty simple bash script which I wrote for my daily use.
Script can enable, disable, edit, remove or create from template nginx virtual
hosts.

## Installation ##

Installation is pretty simple: download, make executable and move in any $PATH
directory. I prefer /usr/bin.

    wget https://raw.githubusercontent.com/bosha/nsvhm/master/nsvhm
    chmod +x
    mv nvshm /usr/bin

You can set SUID bit on script, but I don't recomend do this because I not
enough test it.

## Configuration ##

In most systems script will work out of the box without any configuration. But,
if you placed nginx configuration in non-standart directory or/and
"available"/"enabled" directories too - you need change those variables:

* NGINX_DIR
* NGINX_CONF
* EN_HOSTDIR
* AV_HOSTDIR

## Using ##

First of all - see all available options:

    user@hostname:~$ nsvhm --help
    usage: "nsvhm" [options]

    Simple script for managing nginx virtual hosts.

    OPTIONS:
    -h, --help              Show this help
    -r, --restart           Restart nginx after host managing
    -f, --force             Force nginx configuration testing
    -v, --vhost             Virtual host name
    -e, --enable            Enable virtual host
    -d, --disable           Disable virtual host
    -l, --list              List files in directory. Can be combined with -e or -d options
    -c, --create            Create virtual host from template
    -t, --template          Specify template file
    -E, --edit              Edit host
    -R, --remove            Remove host
    user@hostname:~$

### Enable virtual host ###

    user@hostname:~$ nsvhm --enable
    1) /etc/nginx/sites-available/default
    2) /etc/nginx/sites-available/example.com
    Type a host number to enable or any other key to exit: 6
    Virtual host file {example.com} succesful enabled!
    Configuration test - [OK]
    Restart nginx to apply changes? (y/n): y
    Nginx successful restarted.
    user@hostname:~$

### Disable virtual host ###

    user@hostname:~$ nsvhm --disable
    1) /etc/nginx/sites-available/default
    2) /etc/nginx/sites-available/example.com
    Type a host number to disable or any other key to exit: 2
    {example.com} successful disabled!
    Restart nginx to apply changes? (y/n): y
    Nginx successful restarted.
    user@hostname:~$

### Edit virtual host ###

Will open virtual host for editing using $EDITOR:

    user@hostname:~$ nsvhm --edit
    1) /etc/nginx/sites-available/default
    2) /etc/nginx/sites-available/example.com
    Type a host number to edit or any other key to exit: 2
    Restart nginx to apply changes? (y/n): y
    Nginx successful restarted.
    user@hostname:~$


### Create virtual host ###

Template can be specified inside script in VHOST_TEMPLATE variable, or can be
specified with --template option:

    user@hostname:~$ nsvhm --create
    Vhost name not defined. Enter one (Ctrl-C to abort): example.com
    Root directory does not exists. Wanna create one? (y/n): y
    Directory /var/www/example.com/html successful created.
    error_log directory does not exists. Wanna create one?  (y/n): y
    Directory /var/www/example.com/log successful created.
    Want change owner for newly created root directory?  (y/n): y
    Successful changed owner
    Vhost file successful created
    Want enable newly created vhost? (y/n): y
    Virtual host file {example.com} succesful enabled!
    Configuration test - [OK]
    Restart nginx to apply changes? (y/n): y
    Nginx successful restarted.
    user@hostname:~$

### Remove virtual host ###

    user@hostname:~$ nsvhm --remove
    1) /etc/nginx/sites-available/default
    2) /etc/nginx/sites-available/example.com
    Type a host number to remove or any other key to exit: 4
    Are you sure you want remove {example.com}? (y/n): y
    Successful deleted {example.com} from enabled vhosts
    Successful deleted {example.com} from available vhosts
    Remove also vhost root directory? (y/n): y
    Successful removed {/var/www/example.com/html} directory
    Restart nginx to apply changes? (y/n): y
    Nginx successful restarted.
    user@hostname:~$

### Other ###

If you want to avoid asking to specify virtual host name - you can specify it
with --vhost option:

    user@hostname:~$ nsvhm --disable --vhost example.com
    {example.com} successful disabled!
    Restart nginx to apply changes? (y/n): y
    Nginx successful restarted.
    user@hostname:~$

Also, if you want to use this script from another - you can use --batch option.
Note that for any questions 'yes' will be chosen as answer, and some options
cannot be specified with command line arguments (for example: root, error_log,
access log directories will be created if not exists), and it cannot be
configured now:

    user@hostname:~$ nsvhm --create --vhost example.com --batch
    Directory /var/www/example.com/html successful created.
    Directory /var/www/example.com/log successful created.
    Successful changed owner
    Vhost file successful created
    Virtual host file {example.com} succesful enabled!
    Configuration test - [OK]
    Nginx successful restarted.
    user@hostname:~$

## Getting help ##

As I mentioned above - script should work out of the box in most modern linux
distributions. However, I used them only in Debian, Ubuntu and CentOS. In those
distributions script worked like a charm. If you encounter problems running this
script in some system - open an issue. I will try to do my best to help you.

## Changelog ##

### Version 0.1 ###
* Initial version
