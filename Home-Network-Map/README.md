# **Home Network Map.**
 ## Overview
 Mapped and documented my home network by using commands in the terminal. I then drew a diagram of my home network in order to understand how my devices connect
 and what the network looks like.

 ## Diagram
![Home network map](https://github.com/EesaRH/Writeups/blob/main/network-diagram.png?raw=true)

## How the addressing works
- Subnet: 192.168.0.0/24, subnet mask 255.255.255.0, 254 usable addresses.
- Default gateway: the booster being 192.268.0.1
- When device joins, DCHP discover sent, returned with DHCP offer by DHCP server. Device then sends DHCP request and is returned with DHCP ack by DHCP server.
  DHCP assigns the ip address, default gateway and the address of a DNS server this way.
- DNS server then translates names like github.com into IP addresses for my PC.

## Findings
My PC is connected to a booster via ethernet, this booster runs its own network. I used Traceroute to confirm there was two hops onto private networks which would be my booster and then main router respectively. This means that traffic is translated twice, pointing to it being a double NAT. I scanned the booster's network and 
found 8 devices other than my PC. The network of the main router is not mapped as I couldn't access its admin page.

## Tests I ran
- `ipconfig -/all` in order to determine my PC's IP address, the subnet mask, default gateway, DNS servers and whether or not DHCP is enabled.
- `tracert 8.8.8.8` to follow the hops made by my PC.
- `ping` sent to the second hop. Returned 100% packet loss but I'm assuming that's because the main router is set to ignore pings.
- `arp -a` and ping sweep which was `for /L %i in (1,1,254) do @ping -n 1 -w 200 192.168.0.%i | find "TTL"` to find all devices connected.

## Troubleshooting
- Typed default gateway into browser however the connection was unsuccessful. I remembered my PC was connected to a booster via ethernet which acts as its own
  network but doesn't have it's own admin page. To confirm I ran `tracert 8.8.8.8` and since the first two hops were private addresses it affirmed that the network   has two layers, the first layer being the booster and the second being the main router.
- I typed the main routers IP into the browser but couldn't connect as the router took "too long to respond". Because of this I resorted to using `arp -a` and
  `tracert` to map the network instead.

## What I'd change
- I would put the booster in access point mode so that the main router is what handles addressing, avoiding double NAT and making the network simpler to monitor
  and manage.

## What I learned
- What double NAT is.
- After further research that I could use the app 'Advanced IP scanner' to determine what the unknown devices were.
- That `arp -a` only shows recently communicated with devices.
