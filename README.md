# godoas
Go implementation of OpenBSD [doas(1)](https://man.openbsd.org/doas).


### Background

Sudo is bloated which causes [issues](https://access.redhat.com/security/cve/cve-2019-14287).
To understand how sudo works, one has to read a 200 page [book](https://www.amazon.com/gp/product/B07WNS9K1L/ref=dbs_a_def_rwt_hsch_vapi_taft_p1_i0) (great book btw).

*There’s a better way: [doas](https://flak.tedunangst.com/post/doas-mastery)!*

Why doas? Because unlike [sudo(1)](https://www.sudo.ws/man/1.8.27/sudo.man.html), [doas(1)](https://man.openbsd.org/doas) makes sense.
