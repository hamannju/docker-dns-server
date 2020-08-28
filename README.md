# docker-dns-server
Docker Compose Setup for a DoH DNS Server with Coredns -> PiHole -> Cloudflared

To run this: 

 Clone the Repo &
 
 $ docker-compose up -d --build

The primary problem I encountered was that I was unable to run the PiHole on another port but 53/udp so I can run CoreDNS in front of it on a single host. By using Docker the PiHole can bind to port 53 on the internal network and CoreDNS can forward the default location to it. This has the added benefit of anonymizing the requests source IPs because it will always be the local address of the CoreDNS container (even though this could be configured in the PiHole, too). 
Finally the PiHole forwards all requests to cloudflared so that they can be resolved using DoH. So nobody can snoop in on our traffic except CloudFlare. It would also be possible (and nicer) to use https://github.com/DNSCrypt/dnscrypt-proxy instead here, but I did not implement this yet.

The Corefile and DNS Zones are exposed via a volume under /config/coredns/Corefile and /config/coredns/zones/. Because Coredns can be configured to reload these files periodically, it is possible to modify and have them loaded into the running container after ~30 seconds. By default the Corefile will forward every request to the PiHole.
The PiHole has its data written into an external volume as well. It is not necessary to modify anything here, it is just for keeping files between updates.

The port bindings on the host are as follows:
- CoreDNS Container: 53/tcp 53/udp (for DNS)
- PiHole Container: 80/tcp 443/tcp (for ad-blocking)
- cloudflared Container: None
