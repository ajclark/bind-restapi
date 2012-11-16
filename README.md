# bind-restapi

A quick and simple RESTful API to BIND, written in Ruby / Sinatra. Provides the ability to add/remove entries with an existing BIND DNS architecture.

I wrote this as a solution to enable our internal Cloud to add/remove machines to DNS by integrating with the DNS architecture that we have today.

## Instructions
	$ ruby dns.rb
    # cd etc/
    # named -c named.conf

### Add a record to DNS:

    $ curl -X POST -H 'Content-Type: application/json' -H 'X-Api-Key: secret' -d '{ "hostname": "host12.apple.com", "ip": "1.1.1.12" }' http://localhost:4567/dns

### Remove a record from DNS:

    $ curl -X DELETE -H 'Content-Type: application/json' -H 'X-Api-Key: secret' -d '{ "hostname": "host12.apple.com", "ip": "1.1.1.12" }' http://localhost:4567/dns

## API
The API supports POST and DELETE methods to add and remove entries, respectively. On a successful POST a 201 is returned. On a successful DELETE a 200 is returned. Duplicate records are never created.

The API can reside on a local *or* remote DNS server.

On a POST request, the API adds **both** the *forward* zone **and** *reverse* in-addr.arpa zone entry as a convenience. 

On a DELETE request, the API removes **both** the *forward* zone **and** *reverse* in-addr.arpa zone entry as a connivence. 

The TTL and other DNS params are hard-coded inside of <code>dns.rb</code>

## Security
The API is protected by way of an API-Key using a custom <code>X-Api-Key</code> HTTP header. The API should also be served over a secure connection. 

## etc
Example named configuration files are included to help get started with integrating <code>dns.rb</code> with BIND.
