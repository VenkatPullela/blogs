# All About Flows
## Flow Switching, Caching, Aging and Tracking
Flows are in fashion again. First L3 switches were flow based. Then ASICs started building full fledged routing tables that allowed them to scale. Flow related functions moved out of switches in to middle boxes with emergence of Network Processors. With recent advances in smart NICs and emergence of DPUs, IPUs flows are again taking the center stage. Programmability and P4 are also adding lot of new and interesting possibilities. 

This blog is a journey down the memory lane to review the history of Flows.

- [Flow Switching, Flow Caching](https://github.com/VenkatPullela/blogs/tree/main/all-about-flows/flow_switching.md)
- [Hardware FLow Table Implementation](https://github.com/VenkatPullela/blogs/tree/main/all-about-flows/flow_table_in_hw.md)
- [Types of Flows](https://github.com/VenkatPullela/blogs/tree/main/all-about-flows/flow_types.md)
- [Aging Flows](https://github.com/VenkatPullela/blogs/tree/main/all-about-flows/flow_aging.md)
- Slow path, Fast Path - TBD
- Memory Considerations - TBD

# References
- [Catalyst 6500 Series Switches Netflow TCAM Utilization Management - Cisco ](https://www.cisco.com/c/en/us/support/docs/switches/catalyst-6500-series-switches/116434-problemsolution-product-00.html)
- [Hardware based Aging Engine - Search engine for forwarding table content addressable memory US Patent 7,274,693](https://patents.google.com/patent/US7274693B1)
- [TCAM based Flow Table - US Patent 7,197,597 ](https://patents.google.com/patent/US7197597B1)

## Lawyer stuff
Netflow belongs to Cisco Systems

Netflow Data Export (NDE) belongs to Cisco Systems

CCIE belongs to Cisco Systems

P4 belongs to P4.org
