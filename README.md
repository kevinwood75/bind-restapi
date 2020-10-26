# Info

root@loadbalancer named]# nsupdate
> update add 55.2.168.192.in-addr.arpa 55 IN PTR test.woodez.net.
> send
> quit
[root@loadbalancer named]# nslookup 192.168.2.55
55.2.168.192.in-addr.arpa	name = test.woodez.net.


[root@loadbalancer named]# more /etc/nsupdate 
update delete loadbalancer.woodez.net A
update delete 213 PTR
update add loadbalancer.woodez.net 21600 IN A 192.168.2.213
update add 213 21600 IN PTR loadbalancer01.woodez.net


http://wiki.bernardino.org/index.php/Update_DNS_with_nsupdate

https://mediatemple.net/community/products/dv/204644500/managing-reverse-dns-records-for-your-server

https://www.dns-school.org/Documentation/bind-arm/man.nsupdate.html










# bind-restapi

A quick and simple RESTful API to BIND, written in Python/Tornado. Provides the ability to add/remove entries with an existing BIND DNS architecture.

Based on the work of https://github.com/ajclark/bind-restapi Rewritten with Python and Tornado.
To daemonize, daemon.py is used: http://www.jejik.com/articles/2007/02/a_simple_unix_linux_daemon_in_python

## Instructions

    $ Customize defines inside bind-restapi.py
    $ python bind-restapi.py start

### Add a record to DNS:

    $ curl -X POST -H 'Content-Type: application/json' -H 'X-Api-Key: secret' -d '{ "hostname": "host.example.com", "ip": "1.1.1.10", "ptr": "yes", "ttl": 86400}' http://localhost:9999/dns

### Remove a record from DNS:

    $ curl -X DELETE -H 'Content-Type: application/json' -H 'X-Api-Key: secret' -d '{ "hostname": "host.example.com"}' http://localhos    t:  9999/dns

## API

The API supports POST and DELETE methods to add and remove entries, respectively. On a successful POST a 201 is returned. On a successful DELETE a 200 is returned. Duplicate records are never created.

The API can reside on a local *or* remote DNS server.

On a POST request, the API adds **both** the *forward* zone **and** *reverse* in-addr.arpa zone entry *ONLY* if **{"ptr": "yes"}** is specified.

On a DELETE request, the API removes **both** the *forward* zone **and** *reverse* in-addr.arpa zone entry as a connivence 

The TTL, portand other DNS params are hard-coded inside of <code>dns.rb</code>

TTL can be overriden from the request with **{"ttl": "$TTL"}**

## Security

The API is protected by way of an API-Key using a custom <code>X-Api-Key</code> HTTP header. The API should also be served over a secure connection. 
