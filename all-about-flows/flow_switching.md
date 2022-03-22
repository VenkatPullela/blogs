# Flow Switching
__Flow switching__ was one of the first successful L3 switching implementations, before ASICs implemented full fledged routing in hardware.

Flow Switching, sometimes referred to as __Netflow Switching__, is based on the concept that the first packet goes through the full functionality 
of the slow path of a router, and subsequent packets can take a faster path if the final results of the features are remembered. 
Since the feature results depend on network layer flow or the 5 tuple (Source IP, Destination IP, Protocol, Source Port, Destination Port) 
a flow cache is constructed with the 5 tuple as the key and the feature results as associated data.

## Flow Caching
In one form of Netflow Switching implementation, called __Shortcut Switching__, a router is front-ended with a switch, 
called __Netflow Switch__ that implements a __Netflow Cache__. Router is connected only to the switch, through many linkes, for example one per VLAN, called milking machine topology, or a single trunk port called rputer on a stick topology. Packets have to go through the switch to reach the router. Over a period routers were embedded in the switch directly, making it a single box L3 switch. In its first impleentation hardware could process packets at 10x the speed as software, increasing the overall throughput from 180K PPS to 1.8M PPS.

![Router On A Stick](https://github.com/VenkatPullela/blogs/blob/main/assets/roast.png)

### Netflow switch implementation
1. For each packet going to the router, if there is no matching cache entry, create an entry, with 5 tuple as the key. Mark it as a potential __Candidate__ for converting it into a shortcut.
![Candidate](https://github.com/VenkatPullela/blogs/blob/main/assets/candidate.png)
2. For each packet coming from the router, is a potential __Enabler__ of a shortcut. if there is a Candidate cache entry, it is made into a shortcut, by learning how the router modified the packets. 
![Enabler](https://github.com/VenkatPullela/blogs/blob/main/assets/enabler.png)
3. For each packet going to the router, if there is a matching shortcut entry(indicated by s bit in the control, see Figure below), use the __Shortcut__, by applying the learned packet transformation information.
![Shortcut](https://github.com/VenkatPullela/blogs/blob/main/assets/shortcut.png)

Since packets are seen by both the Netflow switch and the router, we may double or triple count packets. 
I designed __Netflow Data Export(NDE)__, version 7, to correlate traffic export from the switch and the router. 

## Lawyer stuff
Netflow belongs to Cisco Systems

Netflow Data Export (NDE) belongs to Cisco Systems

CCIE belongs to Cisco Systems

P4 belongs to P4.org

---

[ Back to blog](https://github.com/VenkatPullela/blogs/tree/main/all-about-flows)

[Next Flow Types >> ](https://github.com/VenkatPullela/blogs/tree/main/all-about-flows/flow_types.md)
