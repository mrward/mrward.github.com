---
layout: post
title: "Proxy Testing a Mac Application with Fiddler or Apache"
date: 2018-10-14 12:38
comments: true
categories: proxy
---

Investigating a problem with Visual Studio for Mac not validating licenses when a proxy server
is being used resulted in setting up a test proxy server on the local network.

Here we look at using [Fiddler](https://www.telerik.com/fiddler) or [Apache](https://httpd.apache.org) as
a proxy server in order to test a Mac client application can make web requests through the proxy. Enabling
Basic and NTLM authentication on the proxy server will be covered, as well as using the proxy without
any authentication.

The proxy server will run on a separate Windows machine. Using a separate machine
allows us to check that the Mac client application is not bypassing the proxy server and making direct
web requests to any server. This check can be done by configuring the local firewall to block all direct
connections out from the Mac machine but allow connections from the proxy server machine.

 - Mac High Sierra 10.13
 - Fiddler 5.0 on Windows
 - Apache httpd 2.4.35 on Windows

## Fiddler Proxy - No authentication

To enable the Mac to use Fiddler as its proxy first configure Fiddler to allow remote connections.
This can be done by open the Tools menu, selecting Options, opening the Connections tab, and
checking [Allow remote computers to connect](http://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/MonitorRemoteMachine). 

{% img /images/blog/ProxyTestingMacApplication/FiddlerRemoteConnections.png 'Fiddler allow remote connections' 'Fiddler allow remote connections' %}

Then on the Mac open System Preferences - Network. Then for the active network select the
Advanced button and open the Proxies tab. Enter the Fiddler's machine IP address and
port into the Web Proxy (HTTP) and Secure Web Proxy (HTTPS) sections, and ensure both
of these are checked.

{% img /images/blog/ProxyTestingMacApplication/MacNetworkWebProxySettings.png 'Mac web proxy settings' 'Mac web proxy settings' %}

Finally click OK and Apply to enable the proxy settings.

Now http and https requests should be sent through Fiddler. A simple way to test this is
to open Safari and open a web page.

{% img /images/blog/ProxyTestingMacApplication/FiddlerNuGetHttpsRequest.png 'Fiddler https request recorded' 'Fiddler https request recorded' %}

## Fiddler Proxy - Basic authentication

To enable Basic proxy authentication in Fiddler open the Rules menu and ensure that
Require Proxy Authentication is checked.

{% img /images/blog/ProxyTestingMacApplication/FiddlerProxyAuthEnabled.png 'Fiddler require proxy authentication option' 'Fiddler require proxy authentication option' %}

This will result in Fiddler requiring Basic authentication with a username '1' and a
password '1'.

Unfortunately it seems that the native Mac networking API does not work with Fiddler
for https requests but works for non-secure http requests. Opening a https url in Safari will cause the Mac to prompt for
the proxy credentials, after entering the credentials, Safari will display an error page:

{% img /images/blog/ProxyTestingMacApplication/SafariProxyServerError.png 'Safari 310 proxy error' 'Safari 310 proxy error' %}

The credentials are stored in the Mac's key chain, named after the IP address of the proxy server, and if you open the
Mac's Network settings you will see the Web Proxy and Secure Web Proxy settings now have a username and password
associated with them.

It seems that for a https request the Mac sends the request with two keep alive headers:

 - Connection: keep-alive
 - Proxy-Connection: keep-alive

Fiddler responds with a 407 proxy authentication required response and a close
header.

 - Connection: close

{% img /images/blog/ProxyTestingMacApplication/FiddlerProxyAuthHttpsRequestDenied.png 'Fiddler https request denied' 'Fiddler https request denied' %}

Since the keep-alive is not being honoured by Fiddler the web request fails from the Mac fails and an kCFErrorDomainCFNetwork 310
error occurs. This maps to the kCFErrorHTTPSProxyConnectionFailure status code.

Other applications that do not use the native Mac networking API, such as Chrome,
will work with Fiddler and will prompt for credentials.

Unfortunately you currently cannot use Fiddler with basic proxy authentication enabled to test
a Mac application making https requests. As an alternative we will take a look at using
Apache.

## Apache Proxy - No authentication



## Apache Proxy - Basic authentication


## Apache Proxy - NTLM authentication

