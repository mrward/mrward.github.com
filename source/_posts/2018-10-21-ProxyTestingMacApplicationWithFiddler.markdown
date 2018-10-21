---
layout: post
title: "Proxy Testing a Mac Application with Fiddler"
date: 2018-10-21 12:38
comments: true
categories: proxy
---

Here we look at using [Fiddler](https://www.telerik.com/fiddler) as
a proxy server in order to test a Mac client application, such as Safari or Visual Studio for Mac, can make web 
requests through the proxy. Enabling Basic proxy authentication with Fiddler will be covered, as well as using 
the Fiddler as a proxy without any authentication.

Fiddler will run on a separate Windows machine. Using a separate machine
allows us to check that the Mac client application is not bypassing the proxy and making direct
web requests to any server. This check can be done by configuring the local firewall to block all direct
connections out from the Mac machine but allow connections from the machine running Fiddler.

 - Mac High Sierra 10.13
 - Fiddler 5.0 on Windows

## Fiddler Proxy - No authentication

To enable the Mac to use Fiddler as its proxy first configure Fiddler to allow remote connections.
This can be done by open the Tools menu, selecting Options, opening the Connections tab, and
checking [Allow remote computers to connect](http://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/MonitorRemoteMachine). 

{% img /images/blog/ProxyTestingMacApplicationWithFiddler/FiddlerRemoteConnections.png 'Fiddler allow remote connections' 'Fiddler allow remote connections' %}

Configuring the Mac to use Fiddler as its proxy server can be done by opening System Preferences - Network.
For the active network select the Advanced button and open the Proxies tab. Enter the Fiddler's machine IP address and
port into the Web Proxy (HTTP) and Secure Web Proxy (HTTPS) sections, and ensure both
of these are checked.

{% img /images/blog/ProxyTestingMacApplicationWithFiddler/MacNetworkWebProxySettings.png 'Mac web proxy settings' 'Mac web proxy settings' %}

Finally click OK and Apply to enable the proxy settings.

Now http and https requests should be sent through Fiddler. A simple way to test this is
to open Safari and open a web page.

{% img /images/blog/ProxyTestingMacApplicationWithFiddler/FiddlerNuGetHttpsRequest.png 'Fiddler https request recorded' 'Fiddler https request recorded' %}

## Fiddler Proxy - Basic authentication

To enable Basic proxy authentication in Fiddler open the Rules menu and ensure that
Require Proxy Authentication is checked.

{% img /images/blog/ProxyTestingMacApplicationWithFiddler/FiddlerProxyAuthEnabled.png 'Fiddler require proxy authentication option' 'Fiddler require proxy authentication option' %}

This will result in Fiddler requiring Basic authentication with a username '1' and a
password '1'.

Unfortunately it seems that the native Mac networking API does not work with Fiddler
for https requests but will work for non-secure http requests. Opening a web page in Safari will cause the Mac to prompt for
the proxy credentials.

{% img /images/blog/ProxyTestingMacApplicationWithFiddler/ProxyAuthenticationRequiredDialog.png 'Mac proxy authentication required dialog' 'Mac proxy authentication required dialog' %}

Clicking the System Preferences button will show open a dialog where you can enter the credentials for the proxy.

{% img /images/blog/ProxyTestingMacApplicationWithFiddler/ProxyCredentialsDialog.png 'Mac proxy authentcation required - credentials dialog' 'Mac proxy authentication required - credentials dialog' %}

Entering the credentials will result in the Mac's key chain being updated with the credentials associated with the proxy's address.

{% img /images/blog/ProxyTestingMacApplicationWithFiddler/ProxyCredentialsMacKeyChain.png 'Proxy credentials stored in Mac key chain' 'Proxy credentials stored in Mac key chain' %}

If you open the Mac's Network settings you will see the Web Proxy and Secure Web Proxy settings now have a username and password
associated with them.

{% img /images/blog/ProxyTestingMacApplicationWithFiddler/MacNetworkSettingsProxyUserNamePassword.png 'Username and password associated with proxy in network settings' 'Username and password associated with proxy in network settings' %}

Safari will display an error page when accessing a web page over https when the Mac is configured to use Fiddler's basic authentation proxy.

{% img /images/blog/ProxyTestingMacApplicationWithFiddler/SafariProxyServerError.png 'Safari 310 proxy error' 'Safari 310 proxy error' %}

It seems that for a https request the Mac sends the request with two keep alive headers:

 - Connection: keep-alive
 - Proxy-Connection: keep-alive

Fiddler responds with a 407 proxy authentication required response and a close
header.

 - Connection: close

{% img /images/blog/ProxyTestingMacApplicationWithFiddler/FiddlerProxyAuthHttpsRequestDenied.png 'Fiddler https request denied' 'Fiddler https request denied' %}

Since the keep-alive is not being honoured by Fiddler the web request sent from the Mac fails and an kCFErrorDomainCFNetwork 310
error occurs. This maps to the kCFErrorHTTPSProxyConnectionFailure status code.

Other applications that do not use the native Mac networking API, such as the Google Chrome web browser,
will work with Fiddler and will prompt for credentials.

Unfortunately you currently cannot use Fiddler with basic proxy authentication enabled to test
a Mac application making https requests. As an alternative you can use another proxy server, such as Apache.

