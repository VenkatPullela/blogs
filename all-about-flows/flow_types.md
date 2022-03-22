# Flow types
Network traffic and the and the flows vary based on the nature of applications they support. 
- Interactive
- Live Video
- Live Audio
- Data Backup
- Service Mesh
- AI Training
- Financial Transactions
- Gaming
- And more ...

Each of these differ in the packet rates, flow arrival rates, inter packet intervals, burstiness, security concerns etc. 
These differences have huge implications for Flow switching architectures. These flow types need to be identified, 
before business logic can be applied either uniformly or to apply differentiated features. Building hardware to match 
the latency, buffering, performance and congestion control mechanisms required for each of these types is difficult. 
Different traffic mixes at different places in the network(PINs) and places in the cloud(PICs) makes it even more difficult. 
Programmable hardware, driven by the emergence of P4 is allowing new architectures to be experimented with at a rapid pace. 
This is an exciting area with a lot of activity and innovation at the time of this writing in 2022. 

## Lawyer stuff
Netflow belongs to Cisco Systems

Netflow Data Export (NDE) belongs to Cisco Systems

CCIE belongs to Cisco Systems

P4 belongs to P4.org

---

[ Back to blog](https://github.com/VenkatPullela/blogs/tree/main/all-about-flows)

[Next Flow Aging >> ](https://github.com/VenkatPullela/blogs/tree/main/all-about-flows/flow_aging.md)
