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
