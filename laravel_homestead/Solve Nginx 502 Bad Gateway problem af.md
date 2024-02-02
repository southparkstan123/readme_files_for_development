# Solve Nginx 502 Bad Gateway problem after upgrade Laravel Homestead

## Change the PHP version

1. ```vagrant ssh``` to access the VM.

2. On VM shell, ```sudo update-alternatives --config php``` and the choices shows as follows:

Example:
```bash
There are 4 choices for the alternative php (providing /usr/bin/php).

  Selection    Path             Priority   Status
------------------------------------------------------------
* 0            /usr/bin/php7.2   72        auto mode
  1            /usr/bin/php5.6   56        manual mode
  2            /usr/bin/php7.0   70        manual mode
  3            /usr/bin/php7.1   71        manual mode
  4            /usr/bin/php7.2   72        manual mode
Press <enter> to keep the current choice[*], or type selection number:
```

3. Select the version of PHP as desired

## Restart Nginx

1. ```cd /var/log/nginx && sudo service php<VERSION_OF_PHP>-fpm restart```

2. Access the ```http://<YOUR_TEST_DOMAIN>```

### Remark

If you want to run up the peoject with older version of PHP after restart Homestead, please do above steps. Because New Homestead select ```PHP 8.2``` or above as the default version.