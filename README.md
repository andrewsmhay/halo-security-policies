##CloudPassage Halo Policy: CVE-2012-1823 - Apache / PHP5.x Remote Code Execution Exploit

###About
On Tuesday, October 29th, 2013, exploit author <a href="http://www.exploit-db.com/author/?a=1856" target="new">Kingcope</a> released exploit code targeting a known vulnerability in Apache and PHP that allowed for remote code execution under certain conditions. According to the exploit's author:

> This is a code execution bug in the combination of Apache and PHP. On Debian and Ubuntu the vulnerability is 
> present in the default install of the php5-cgi package. 

> When the php5-cgi package is installed on Debian and Ubuntu or php-cgi is installed manually the php-cgi binary 
> is accessible under /cgi-bin/php5 and /cgi-bin/php. The vulnerability makes it possible to execute the binary 
> because this binary has a security check enabled when installed with Apache http server and this security check 
> is circumvented by the exploit.
>
> When accessing the php-cgi binary the security check will block the request and will not execute the binary.
>
> In the source code file sapi/cgi/cgi_main.c of PHP we can see that the security check is done when the php.ini 
> configuration setting cgi.force_redirect is set and the php.ini configuration setting cgi.redirect_status_env is 
> set to no.
>
> This makes it possible to execute the binary bypassing the Security check by setting these two php.ini settings. 
> Prior to this code for the Security check getopt is called and it is possible to set cgi.force_redirect to zero 
> and cgi.redirect_status_env to zero using the -d switch. If both values are set to zero and the request is sent 
> to the server php-cgi gets fully executed and we can use the payload in the POST data field to execute arbitrary 
> php and therefore we can execute programs on the system.
>
> apache-magika.c is an exploit that does exactly the prior described. It does support SSL.

More information on the exploit, as well as the code, can be found at <a href="http://www.exploit-db.com/exploits/29290/" target="new">http://www.exploit-db.com/exploits/29290/</a>.

####Affected and tested versions
* PHP 5.3.10
* PHP 5.3.8-1
* PHP 5.3.6-13
* PHP 5.3.3
* PHP 5.2.17
* PHP 5.2.11
* PHP 5.2.6-3
* PHP 5.2.6+lenny16 with Suhosin-Patch
####Affected versions
* PHP prior to 5.3.12
* PHP prior to 5.4.2
####Unaffected versions
* PHP 4 - getopt parser unexploitable
* PHP 5.3.12 and up
* PHP 5.4.2 and up
Unaffected versions are patched by CVE-2012-1823.

###CloudPassage Policy

To help users detect if their current Apache and PHP installations are succeptible to this attack, <b>the CVE-2012-1823 - Apache / PHP5.x Remote Code Execution Exploit</b> configuration policy was created.

###Rules and Checks
It should be noted that the following rules and checks could serve as a potential indicator of compromise (IOC). That being said, an alert on a true possitive on an individual check will likely not serve as the sole indicator of vulnerability, but it should still be investigated.

<b>System Configuration > Vulnerable PHP version possibly detected</b>
<ul><li>Several File String Presence checks exist to see if one of the listed php5 versions was installed using the package manager by inspecting /var/log/dpkg.log</li></ul>

<b>Software Configuration > cgi.force_redirect</b>
<ul><li>Contains a File String Presence check to see if the cgi.force_redirect setting is enabled within the three most common php.ini files on the system - /etc/php5/cli/php.ini, /etc/php5/cgi/php.ini,and /etc/php5/apache2/php.ini.</li></ul>

<b>Software Configuration > cgi.redirect_status_env</b>
<ul><li>Contains a File String Presence check to see if the cgi.redirect_status_envsetting is enabled within the three most common php.ini files on the system - /etc/php5/cli/php.ini, /etc/php5/cgi/php.ini,and /etc/php5/apache2/php.ini.</li></ul>

###Contact

To provide any feedback or ask any questions please reach out to Andrew Hay on Twitter at <a href="http://twitter.com/andrewsmhay" target="new">@andrewsmhay</a>.
