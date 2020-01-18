# DNS Pinning for Human Rights

This repo contains various DNS server config files meant to perform
DNS pinning, primarily for blocking at a DNS level racist,
xenophobic, transphobic, and other human rights-violating domains.

As DNS pinning can also be used for good (much like TLS certificate
pinning), rules for providing safety/security of human
rights-promoting are encouraged.

All content in this repo is in the public domain.

The directory structure is as follows:

* `/blocking`: Config files for domains that should be blocked.
* `/blocking/unbound`: Config files for `unbound` which can be
  included via the `include:` directive.
* `/protecting`: Config files for domains that should be protected.

## History

This repo was started out of the idea to perform a
dynamically-updating list of DNS rules promoting Human Rights. Upon
auditing DNS server hardening, I found attempts to resolve a certain
neo-nazi social network and decided to take action. There will be no
nazis on my network.

This spawned a [Mastodon thread](https://bsd.network/@lattera/103500720691059078)
that brought the idea: a Human Rights-centric PiHole-like list of DNS
rules.

## Example production deployment

On a production HardenedBSD + Unbound setup, I have the following
config:

```plain
# cd /usr/local/etc/unbound
# git clone \
    https://git-01.md.hardenedbsd.org/shawn.webb/dnspinning-for-human-rights.git \
    dnspinning
# cat unbound.conf
server:
	verbosity: 1
	interface: 0.0.0.0
	access-control: 10.0.0.0/8 allow

	###########################
	#### Generic hardening ####
	###########################
	harden-algo-downgrade: yes
	harden-glue: yes
	harden-referral-path: yes
	harden-short-bufsize: yes
	hide-identity: yes
	use-caps-for-id: yes
	ignore-cd-flag: yes

	###################################
	#### Validator-based hardening ####
	###################################
	val-clean-additional: yes
	val-permissive-mode: no

	#################################################################
	#### Prevent DNS rebinding attacks by stripping private IPs #####
	#################################################################
	private-address: 10.0.0.0/8
	private-address: 172.16.0.0/12
	private-address: 192.168.0.0/16
	private-address: 169.254.0.0/16
	private-address: fd00::/8
	private-address: fe80::/10
	private-address: ::ffff:0:0/96

	unwanted-reply-threshold: 10000000

	module-config: "validator iterator"
	auto-trust-anchor-file: "/usr/local/etc/unbound/root.key"

#######################################
#### Piss off nazis on our network ####
#######################################
include: /usr/local/etc/unbound/dnspinning/blocking/unbound/1.x/nazis.conf
```

When I want to update the deployed rules:

```plain
# cd /usr/local/etc/unbound/dnspinning
# git pull
# service unbound reload
```
