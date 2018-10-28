---
layout: post
title: "Proxy Testing a Mac Application with Apache"
date: 2018-10-28 12:00
comments: true
categories: proxy
---

Here we look at using [Apache](http://httpd.apache.org/) as
a proxy server in order to test that a Mac client application, such as Safari or Visual Studio for Mac, can make web 
requests through a proxy. Enabling Basic and NTLM proxy authentication with Apache will be covered, as well as using 
the Apache as a proxy without any authentication.

Apache will run on a separate Windows machine. Using a separate machine
allows us to check that the Mac client application is not bypassing the proxy and making direct
web requests to any server. This check can be done by configuring the local firewall to block all direct
connections out from the Mac machine but allow connections from the machine running Apache.

 - Mac High Sierra 10.13
 - [Apache 2.4.35 on Windows](https://www.apachehaus.com/cgi-bin/download.plx)

## Apache Proxy - No authentication

Unzip Apache and extract the files to c:\Apache24\ which is the default ServerRoot configured in the conf\httpd.conf
file.

    Define SRVROOT "/Apache24"

In the httpd.config file configure the port to be used by the Apache server if required. 
By default port 80 is used. Here we will be using port 8888.

    Listen 8888
    ServerName localhost:8888

[mod_proxy](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html), 
[mod_proxy_connect](https://httpd.apache.org/docs/2.4/mod/mod_proxy_connect.html) and
[mod_proxy_http](https://httpd.apache.org/docs/2.4/mod/mod_proxy_http.html) should be loaded.
Edit the http.conf file and ensure these modules are loaded.

    LoadModule proxy_module modules/mod_proxy.so
    LoadModule proxy_connect_module modules/mod_proxy_connect.so
    LoadModule proxy_http_module modules/mod_proxy_http.so

Add a VirtualHost and Proxy section to the httpd.conf file.

    <VirtualHost *>
         ProxyRequests On
    
        <Proxy *>
            Require all granted
        </Proxy>
    </VirtualHost>

To run Apache, open a command prompt, change into the Apache24\bin directory, then run httpd.exe

    cd c:\Apache24\bin
    httpd.exe

On running Apache for the first time you will see a Windows Firewall dialog.

{% img /images/blog/ProxyTestingMacApplicationWithApache/FirewallAlertApache.png 'Windows Firewall alert for Apache' 'Windows Firewall alert for Apache' %}

Allow Apache to access both the private and public networks.

Configuring the Mac to use Apache as its proxy server can be done by opening System Preferences - Network.
For the active network select the Advanced button and open the Proxies tab. Enter the machine IP address
where Apache is running and its port into the Web Proxy (HTTP) and Secure Web Proxy (HTTPS) sections, and ensure both
of these are checked.

{% img /images/blog/ProxyTestingMacApplicationWithApache/MacNetworkWebProxySettings.png 'Mac web proxy settings' 'Mac web proxy settings' %}

Finally click OK and Apply to enable the proxy settings.

Now http and https requests should be sent through Apache. A simple way to test this is
to open Safari and view a web page. In the C:\Apache24\logs\access.log file you should see entries for the
sites accessed.

    192.168.1.100 - - [28/Oct/2018:12:18:47 +0000] "GET http://neverssl.com/ HTTP/1.1" 200 1183
    192.168.1.100 - - [28/Oct/2018:12:18:47 +0000] "GET http://neverssl.com/favicon.ico HTTP/1.1" 200 124
    192.168.1.100 - - [28/Oct/2018:12:18:41 +0000] "CONNECT www.nuget.org:443 HTTP/1.1" 200 -

## Apache Proxy - Basic authentication

To enable Basic authentication change the Proxy section in the httpd.conf file as shown below.

    <VirtualHost *>
         ProxyRequests On
    
        <Proxy *>
            AuthType Basic
            AuthName "Authentication required"
            AuthBasicProvider file
            AuthUserFile proxy-password.file
            Require valid-user
        </Proxy>
    </VirtualHost>

Here we will be using [htpasswd.exe](https://httpd.apache.org/docs/2.4/programs/htpasswd.html) to create a file to
store the username and password required to authenticate against
the proxy server. htpasswd.exe is in the Apache bin directory. Run the following to generate the proxy-password.file,
replacing user with the username to be used with the proxy.

    htpasswd.exe -c proxy-password.file user

Enter the password when prompted.

Copy the proxy-password.file to the Apache server's root directory c:\Apache24

To make it easier to see the basic auth header being sent you may want to enable
[forensic logging](https://httpd.apache.org/docs/2.4/mod/mod_log_forensic.html) in Apache.
To do this add a LoadModule to the httpd.conf file.

    LoadModule log_forensic_module modules/mod_log_forensic.so

Define the path to the forensic log file using an IfModule.

    <IfModule log_forensic_module> 
        ForensicLog "logs/forensic.log"
    </IfModule> 

Run httpd.exe from the Apache24\bin directory.

When connecting through the proxy
for the first time the Mac will show a dialog asking for credentials.

{% img /images/blog/ProxyTestingMacApplicationWithApache/ProxyAuthenticationRequiredDialog.png 'Mac proxy authentication required dialog' 'Mac proxy authentication required dialog' %}

Clicking the System Preferences button will show open a dialog where you can enter the credentials for the proxy.

{% img /images/blog/ProxyTestingMacApplicationWithApache/ProxyCredentialsDialog.png 'Mac proxy authentcation required - credentials dialog' 'Mac proxy authentication required - credentials dialog' %}

Entering the credentials will result in the Mac's key chain being updated with the credentials associated with the proxy's address.

{% img /images/blog/ProxyTestingMacApplicationWithApache/ProxyCredentialsMacKeyChain.png 'Proxy credentials stored in Mac key chain' 'Proxy credentials stored in Mac key chain' %}

If you open the Mac's Network settings you will see the Web Proxy and Secure Web Proxy settings now have a username and password
associated with them.

{% img /images/blog/ProxyTestingMacApplicationWithApache/MacNetworkSettingsProxyUserNamePassword.png 'Username and password associated with proxy in network settings' 'Username and password associated with proxy in network settings' %}

In the Apache access.log you should see the requests being made and a Proxy authentication 407 status code.

    192.168.1.100 - - [28/Oct/2018:12:44:47 +0000] "CONNECT www.nuget.org:443 HTTP/1.1" 407 415
    192.168.1.100 - - [28/Oct/2018:12:44:53 +0000] "GET http://neverssl.com/ HTTP/1.1" 407 415

In the forensic.log file you should see the basic proxy authorization header being used by the Mac.

    GET http%3a//neverssl.com/ HTTP/1.1|Host:neverssl.com|Connection:keep-alive|Proxy-Connection:keep-alive|Upgrade-Insecure-Requests:1|Proxy-Authorization:Basic MTox|Accept:text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8|User-Agent:Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/12.0 Safari/605.1.15|Accept-Language:en-gb|DNT:1|Accept-Encoding:gzip, deflate
    CONNECT www.nuget.org%3a443 HTTP/1.1|Host:www.nuget.org%3a443|User-Agent:Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/12.0 Safari/605.1.15|Connection:keep-alive|Proxy-Authorization:Basic MTox|Proxy-Connection:keep-alive

## Apache Proxy - NTLM authentication

In order to enable NTLM authentication the [NTLM authentication module](https://github.com/TQsoft-GmbH/mod_authn_ntlm) needs to be downloaded.
Download [Mod Auth NTLM for Apache 2.4.x x64 - mod_authn_ntml.zip](https://www.apachehaus.com/cgi-bin/download.plx),
extract the files and copy the mod_auth_ntlm.so file to the Apache24\modules directory.

Add a LoadModule to the httpd.conf file for the mod_authn_ntlm.so file.

    LoadModule auth_ntlm_module modules/mod_authn_ntlm.so

Change the VirtualHost Proxy section as shown below:

    <VirtualHost *>
        ProxyRequests On
    
        <Proxy *>
            AuthType SSPI
            AuthName "Auth required"
            NTLMAuth On
            NTLMAuthoritative On
    
            <RequireAll>
                <RequireAny>
                    Require valid-user
                </RequireAny>
    
                <RequireNone>
                    Require user "ANONYMOUS LOGON"
                    Require user "NT AUTHORITY\ANONYMOUS LOGON"
                </RequireNone>
            </RequireAll>
        </Proxy>
    </VirtualHost>

The username and password used to authenticate against the proxy needs to match an account on the Windows machine.
As with basic authentication, the Mac will prompt for credentials if they do not match
what is stored in the key chain for the proxy.

Run httpd.exe and then use Safari to open a web site to check web requests are being sent through the proxy.
You should see the Windows account being used to authenticate in Apache's access.log.

    192.168.1.100 - - [28/Oct/2018:13:09:19 +0000] "GET http://neverssl.com/ HTTP/1.1" 407 415
    192.168.1.100 - windows\\useraccount [28/Oct/2018:13:09:19 +0000] "GET http://neverssl.com/ HTTP/1.1" 304 -
    192.168.1.100 - - [28/Oct/2018:13:09:38 +0000] "CONNECT www.nuget.org:443 HTTP/1.1" 407 415
    192.168.1.100 - windows\\useraccount [28/Oct/2018:13:09:12 +0000] "CONNECT www.nuget.org:443 HTTP/1.1" 200 -

The forensic log should show that the NTLM header is being used by the Mac.

    CONNECT www.nuget.org%3a443 HTTP/1.1|Host:www.nuget.org%3a443|User-Agent:Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/12.0 Safari/605.1.15|Connection:keep-alive|Proxy-Authorization:NTLM T...|Proxy-Connection:keep-alive
    GET http%3a//neverssl.com/ HTTP/1.1|Host:neverssl.com|Proxy-Connection:keep-alive|Proxy-Authorization:NTLM T...|If-Modified-Since:Thu, 14 Jun 2018 00%3a16%3a40 GMT|Accept:text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8|Accept-Language:en-gb|Accept-Encoding:gzip, deflate|Content-Length:0|User-Agent:Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/12.0 Safari/605.1.15|Connection:keep-alive|Upgrade-Insecure-Requests:1|DNT:1
