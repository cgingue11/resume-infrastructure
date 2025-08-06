# This repository contains the relavant IaC to host my resume
I want to first cover the bits that are missing from this repo.

1. Pangolin

If you're viewing my resume over the internet, you received a signed URL to my web server. This is handled by Pangolin which uses wireguard tunnels under the hood to connect internet traffic to my homelab. I use this for a few reasons. A) Authentication in front of any of my exposed services. B) Layer 7 networking to get around CGNAT limitations. C) A public, static IP for internet traffic to use. With CGNAT, I do not have an IP that I could port forward through. I use a racknerd VPS to host my Pangolin server, and a docker container newt client for connection to my internal services.
2. The third node in my k3s cluster. 

Quorum is needed for a highly available kubernetes cluster. I use a Proxmox VM running Debian 12 as the third node in this cluster. This is mostly because I use thinkcentre tiny machines for my other 2 nodes and I didn't want to buy a third.
3. DNS

At this time, I have not dove into an IaC based DNS server. Externally, I use cloudflare to route subdomains to my VPS. Internally, I run an adguard server on my OPNsense machine for local DNS.
4. SSL Certificates

Externally, pangolin handles SSL termination and makes it *just work*. Internally, I use a docker one-shot job that renews a wildcard SSL certificate using Let'sEncrypt DNS challenges. This runs nightly with a cron job configured on my docker host.

# What's in this repo if all that is missing?
1. NixOS configurations

NixOS is an immutable linux operating system built from configuration files. I run NixOS on bare metal on my thinkcentre tiny k3s cluster nodes. I thought it was a very fun idea to have my kubernetes cluster entirely representable with text files. Of course, as previously mentioned this is only so true as one of my nodes is running debian 12 in a proxmox virtual machine.
2. Kubernetes Resources

The nginx application, service and ingress rule are all included in the k3s directory. Additionally, the index.html that gets mounted from a config map is included as well.
