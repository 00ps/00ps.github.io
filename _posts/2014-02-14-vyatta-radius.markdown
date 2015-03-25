---
layout: post
title:  "Brocade Vyatta with an External RADIUS"
date:   2014-02-14 10:18:00
categories: sysadmin
tags:
  - aws
  - sysadmin
  - tips 'n tricks
---
##### intro
When it comes to secure your access to an AWS VPC, the [Brocade Vyatta vRouter 5400] offers all the required functionalities in a quickly and easily deployable appliance.

You can get it from the [AWS market place][aws-market] and in a few minutes you have a running firewall/VPN gateway!

##### Vyatta in two words
[Vyatta] is a software-based router, firewall and VPN. It is based on [Debian][debian]. It offer a "CLI" that allow an easy an quick configuration (note: there is also a web interface, but I never seen it in action)

##### the RADIUS trick
The default setup is to use a local directory to authenticate your VPN users, which does not match centralized authentication requirements that you may have.

But! there is an undocumented feature that allows you to user an external [RADIUS][radius] to authenticate your users.


First, you will need to tell openvpn to use the **openvpn-auth-pam** plugin, make sure you pass the following options:

{% highlight bash %}
set interfaces openvpn vtun0 openvpn-option '--plugin
/usr/lib64/openvpn/openvpn-auth-pam.so openvpn --comp-lzo
--client-cert-not-required --username-as-common-name --script-security 2'
{% endhighlight %}

Then, you need to configure [PAM][pam] to use your [RADIUS][radius] server.

1. create `/etc/pam_radius_auth.conf` with the following content:

`
ip.address.of.radius  your_radius_secret
`

2. create `/etc/pam.d/openvpn` with the following:

`
#%PAM-1.0
auth          sufficient      pam_radius_auth.so      debug
account       sufficient      pam_permit.so
`

You are now ready to go!!

[aws-market]: https://aws.amazon.com/marketplace/pp/B009I5TLOE/ref=mkt_ste_l2_nw_f2?nc2=h_l3_n
[brocade]: http://www.brocade.com/products/all/network-functions-virtualization/product-details/5400-vrouter/specifications.page
[radius]: http://en.wikipedia.org/wiki/RADIUS
[vyatta]: http://en.wikipedia.org/wiki/Vyatta
[debian]: http://www.debian.org
[pam]: http://en.wikipedia.org/wiki/Pluggable_authentication_module
