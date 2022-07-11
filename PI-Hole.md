## Install Pi-hole in Ubuntu 21.04

> https://www.virtualizationhowto.com/2021/12/install-pi-hole-in-ubuntu-21-04/

1.  Install lighttpd server
2.  Install Pi-hole

The installation process in Ubuntu 21.04 for Pi-hole is simple and straightforward and only takes a few minutes. The process involves:

### 1. Install lighttpd server

First, let’s install lighttpd server in your Ubuntu 21.04 installation. To do that, use the command below. As a note, Pi-hole will install lighttpd for you. However, I like to have control over installing this manually as I had issues with SSL on a previous build. After installing manually I didn’t have any issues getting SSL implemented. (Will detail this in a future post).

```
sudo apt install lighttpd
```

![Install lighttpd server in Ubuntu 21.04](https://www.virtualizationhowto.com/wp-content/uploads/2021/12/Install-lighttpd-server-in-Ubuntu-21.04.png)

Install lighttpd server in Ubuntu 21.04

### 2. Install Pi-hole

After installing lighttpd server, the next step is installing Pi-hole. To do that, just issue the command:

```
sudo curl -sSL https://install.pi-hole.net | bash
```

This launches a GUI installer of sorts from the command line. Here, we just follow the prompts to install Pi-hole.

[![Beginning the Pihole installation in Ubuntu 21.04](https://www.virtualizationhowto.com/wp-content/uploads/2021/12/Beginning-the-Pihole-installation-in-Ubuntu-21.04.png)](https://www.virtualizationhowto.com/wp-content/uploads/2021/12/Beginning-the-Pihole-installation-in-Ubuntu-21.04.png)

Beginning the Pi-hole installation in Ubuntu 21.04

During the installation, the Pi-hole installer alerts to the need to have a static IP address. This is just basic network best practice. You don’t want to have a DNS server that you are using for DNS sinkholing on a floating dynamic IP as this could potentially change over time as DHCP leases run out.

[![Message about static IP with Pihole](https://www.virtualizationhowto.com/wp-content/uploads/2021/12/Message-about-static-IP-with-Pihole.png)](https://www.virtualizationhowto.com/wp-content/uploads/2021/12/Message-about-static-IP-with-Pihole.png)

Message about static IP with Pi-hole

Pi-hole gives you the option to choose which upstream DNS provider you use. What is an upstream provider? In other words, it is the DNS server that Pi-hole uses to answer your DNS requests. Here I like to use the Cloudflare provider. It seems to always edge out the other big names in the list in terms of performance, etc. I have not had an issue with Cloudflare. However, keep in mind all the other providers will work fine here as well.

As a note, if you have another on-premises DNS provider, you can use your custom DNS server here as well with the Custom option.

[![Select your upstream DNS provider](https://www.virtualizationhowto.com/wp-content/uploads/2021/12/Select-your-upstream-DNS-provider.png)](https://www.virtualizationhowto.com/wp-content/uploads/2021/12/Select-your-upstream-DNS-provider.png)

Select your upstream DNS provider

Another good feature of Pi-hole out of the box is it comes with a default blocklist of sorts. So, you don’t have to install one to get started blocking ads on your network. There are many other good Pi-hole blocklists out there as well that you can take advantage of in your installation. These can be added and customized later.

[![Choose default blocklists built into the installation](https://www.virtualizationhowto.com/wp-content/uploads/2021/12/Choose-default-blocklists-built-into-the-installation.png)](https://www.virtualizationhowto.com/wp-content/uploads/2021/12/Choose-default-blocklists-built-into-the-installation.png)

Choose default blocklists built into the installation

You can choose whether or not you want to install the web admin interface, which I highly recommend.

[![Choose to install the web interface for Pihole](https://www.virtualizationhowto.com/wp-content/uploads/2021/12/Choose-to-install-the-web-interface-for-Pihole.png)](https://www.virtualizationhowto.com/wp-content/uploads/2021/12/Choose-to-install-the-web-interface-for-Pihole.png)

Choose to install the web interface for Pi-hole

You can install the webserver and required PHP modules. Here, you can allow Pi-hole to install Lighttpd for you and the required PHP modules. This will work fine for most. As I mentioned earlier, I like to install it manually. Pi-hole will make use of your existing installation.

[![Install the lighttpd web server](https://www.virtualizationhowto.com/wp-content/uploads/2021/12/Install-the-lighttpd-web-server.png)](https://www.virtualizationhowto.com/wp-content/uploads/2021/12/Install-the-lighttpd-web-server.png)

Install the lighttpd webserver

Decide how you want to log queries. I am here just accepting the defaults.

[![Choose how you want to log queries](https://www.virtualizationhowto.com/wp-content/uploads/2021/12/Choose-how-you-want-to-log-queries.png)](https://www.virtualizationhowto.com/wp-content/uploads/2021/12/Choose-how-you-want-to-log-queries.png)

Choose how you want to log queries

You can also decide how you want to handle the privacy mode of Pi-hole. You can choose to show or hide everything.

[![Choose a privacy mode for FTL](https://www.virtualizationhowto.com/wp-content/uploads/2021/12/Choose-a-privacy-mode-for-FTL.png)](https://www.virtualizationhowto.com/wp-content/uploads/2021/12/Choose-a-privacy-mode-for-FTL.png)

Choose a privacy mode for FTL

After clicking OK, you will see the installation of Pi-hole begin.

[![Installation begins for Pihole in Ubuntu 21.04](https://www.virtualizationhowto.com/wp-content/uploads/2021/12/Installation-begins-for-Pihole-in-Ubuntu-21.04.png)](https://www.virtualizationhowto.com/wp-content/uploads/2021/12/Installation-begins-for-Pihole-in-Ubuntu-21.04.png)

Installation begins for Pi-hole in Ubuntu 21.04

After a few moments, you should see that Pi-hole is successfully installed. You will see an automatically generated random password for the admin interface. Be sure to note this password. You can change it later as well.

[![Installation of Pihole on Ubuntu 21.04 completes successfully](https://www.virtualizationhowto.com/wp-content/uploads/2021/12/Installation-of-Pihole-on-Ubuntu-21.04-completes-successfully.png)](https://www.virtualizationhowto.com/wp-content/uploads/2021/12/Installation-of-Pihole-on-Ubuntu-21.04-completes-successfully.png)

Installation of Pi-hole on Ubuntu 21.04 completes successfully

After clicking OK, you will see the command to change the admin password, as well as the location for the log file.

[![Review the command line messaging](https://www.virtualizationhowto.com/wp-content/uploads/2021/12/Review-the-command-line-messaging.png)](https://www.virtualizationhowto.com/wp-content/uploads/2021/12/Review-the-command-line-messaging.png)

Review the command line messaging

Start enjoying the freedom from ads on your network!

![Enjoy blocked ads network wide with Pi hole](https://www.virtualizationhowto.com/wp-content/uploads/2021/12/Enjoy-blocked-ads-network-wide-with-Pi-hole.png)

Enjoy blocked ads network wide with Pi hole

## Wrapping Up

Pi-hole is just one of those solutions and open-source projects that get me excited. The use case hits home for just about anyone who is serious about cleaning up their network from ads, tracking, and malware. Pi-hole provides a  [DNS](https://www.virtualizationhowto.com/2021/07/dns-conditional-forwarder-multiple-internal-domains-setup/) sinkhole that works well to protect your network from many different threats. As shown, you can install Pi-hole in Ubuntu 21.04 in just a few minutes. After installing you simply point your clients to your Pi-hole server and enjoy ad-free browsing!
