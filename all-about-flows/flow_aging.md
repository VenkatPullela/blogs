# Aging Flows
Aging is the mechanism to remove the flows from flow tables to make room for new flows. 

Aging of Netflow cache is the most important factor in the performance of Netflow Switching. 
Expired flows can easily fill up the flow cache forcing new flows to take slow path reducing the performance many fold. 

Even though these aging mechanisms were developed to address specific use cases in mind at the time of design, 
they were designed in a generic way to help network engineers use them to create complex mechanisms to deal with ever changing traffic patterns.

## Time stamps 
To assist in aging, typically, two time stamps are maintained in each flow.

__Granularity:__ Most systems maintain a very accurate time, typically in nanoseconds. 
Since aging need not be that fine grain, 
switches maintain a separate timestamp register (running counter) or derive it from system time by ignoring a few least significant bits.
he granularity of this time stamp determines the aging accuracy, billing accuracy and the memory usage.

__Start Time__ of each flow can be stored in the cache for use in auditing and billing purposes. 
It can also be used in some flow aging schemes to determine packet rates of the flow. 
Some simple implementations may choose not to track flow starting time to save precious memory. 

__Last Used Time__ is saved in the cache each time a packet uses the cache entry. 
This can be used for aging schemes such as Least Recently Used(LRU) etc.

__Wrap Around:__ Running counters that are used for time stamps may wrap around, 
depending on the number of bits used and the frequency of updates. 
This needs to be accounted for while computing idle time, start time for a flow.

## Aging Schemes
I designed a few aging schemes and coined terms to identify them, like fast aging, long aging etc. 
Different kinds of aging use different criteria to identify the flows that can be removed from the cache.
As there are many aging schemes and the naming can get confusing, 
it helps to think of these schemes as methods to identify different kinds of flows and handle them appropriately. 
The identified flows may be removed, refreshed or skipped to operateon other flows.
It may also help to think of Fast Aging as Fast Flow Aging, Slow Aging as Slow Flow Aging and so on.

__Normal aging__ is used to identify flows that are inactive for a configurable period of time and remove them. 

__Long Aging__ is used to remove or refresh flows that are in the flow cach for a long time. 
This is done to make sure counter wrap-arounds do not cause mis-calculations of time stamps, packet counters or byte counters. 
See the section on memory for more details on the trade offs.

__Fast Aging__ is used to make sure fast flowing flows are preferred over other flows while caching. 
Fast flows use the cache entry more frequently and hence provide better return on investment of the cache memory. 
This was originally designed to separate video flows vs. rest of the flows so that video flows are preferred for caching. 

Fast aging allows different forms of specifying flow rate to identify fast flowing flows. In its simplest form, 
it checks for a given number of packets switched over a given number of seconds after the flow is created. 
For example, a customer may configure Fast aging with 10 packers and 10 seconds. This will remove any flows that do not switch 10 packets in 10 seconds. 
Video flows that are higher rates, will not match while other flows are aggressively removed. 

Anecdotally, some clever CCIEs used Fast aging to identify some forms of DoS attacks and aggressively remove the connections without packets. 
Since then DoS attacks have evolved as well as new forms of aging are created.

__Slow Aging__ is used to identify flows that are slow to set up a connection or abandon the connection after set up. 
Connection set up without any traffic may be due to DoS attack.

__FIN Aging__ is used to identify flows that saw a FIN packet. Some implementations may have a simple FIN seen attribute, some may have additional ACK seen after FIN  and others may implement a full fledged TCP connection state tracking. In either case, flows which see a FIN are typically aged faster. This could scheme may have a separate timer configuration, or an acceleration factor that reduce the normal aging by a configurable factor. Usually __RST__ bit seen is also clubbed with the FIN aging schemes.

Switching pipeline it self may do the aging of flows for FIN/RST while switching the packet. This may be done without waiting for the Aging state machine find the flow with the FIN/RST bit later. Typically, this is done when there is a slower path to handle subsequent packets, creating space immediately for other flows that may be waiting for space.

__Flowlet Aging__ is used to identify flowlets within a flow. Flows may have a period of high packet rate followed by a gap or a slow packet rate. Connectionless protocols like UDP or transaction processing may form the flowlets usage pattern.

__Aggressive Aging__ is used to identify high occupancy rate of the flow table. Typically this triggers aggressive aging of flows by reducing the timer values by a configurable factor. Once the utilization goes below a certain threshold, original values of aging timers are restored. This may also trigger higher bandwidth allocation for aging engine temporarily.

## Software implementation
Software based aging implementation for hardware based Flow switching implementations, do not provide dependable performance due to bottlenecks in bus bandwidth, and competing regular work loads, affecting overall switching performance. 

## Hardware based aging
Dedicated [Hardware based Aging Engines](https://patents.google.com/patent/US7274693B1) that run in parallel to 
switching provide predictable performance. These engines carve out a fixed/configurable amount of memory bandwidth to evaluate flows for aging. 
They may also steal cycles when there is a gap between packets to clean up the flow tables. They walk through the flow tables in the background to check each flow against the configured aging mechanisms. When they age a flow, or refresh a flow, they collect flow statistics along with time stamps and other associated data, 
and send them to CPU via FIFOs for further processing and export. Some pipelines support direct export of the flow and telemetry data. These automated engines 
also help in spreading the workload in collecting the statistics, and export (Netflow Data Export, Telemetry). This prevents the overflow of limited size 
FIFOs between the pipelines and the CPUs due to a burst of new flows or aging.

## Flow creation

## Aging vs. Replacement

# References
- [Catalyst 6500 Series Switches Netflow TCAM Utilization Management - Cisco ](https://www.cisco.com/c/en/us/support/docs/switches/catalyst-6500-series-switches/116434-problemsolution-product-00.html)
- [Hardware based Aging Engine - Search engine for forwarding table content addressable memorySearch engine for forwarding table content addressable memory
US Patent 7,274,693](https://patents.google.com/patent/US7274693B1)

## Lawyer stuff
Netflow belongs to Cisco Systems

Netflow Data Export (NDE) belongs to Cisco Systems

CCIE belongs to Cisco Systems

P4 belongs to P4.org

---
[Back to blog](https://github.com/VenkatPullela/blogs/tree/main/all-about-flows)

