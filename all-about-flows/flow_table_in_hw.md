# Hardware implementation of flow tables
Hardware based table implementations were in use even from L2 switching days for MAC-VLAN lookups, 
usually as a CAM (Content Addressable Memory). Typically CAMs are implemented in hardware as a Hash table, not to be confised with 
Ternary CAMs that have a different architecture. Flow table impleentations started with similar hashing techniques. 
Hashing tables came with the typical problems of collisions, lower utilization etc. Handling these hash collisions is harder in hardware implementations 
as they may require additional memory accesses, thus affecting uniform performance. In general, the pipeline designs are difficult when there are 
variable number of operations involved for each packet.

Customers usually look for guaranteed hit percentage of 99% or more for a given flow for deterministic capacity planning. This meant, utilization 
could be as low as 25% of  the flow table in certain implementations with external memory and a limited number of access per packet. 
Various techniques were developed to increase the 
utilization by reducing the memory accesses to the external flow tables. By keeping a smaller version of the flow contents on-chip, 
external memory access can be reduced. This is a classic case of memory vs. compute tradeoff. This could reduce the number of memory accesses 
to within the allowed limit, increasing the utilization. While these design
techniques are universal, System On a Chip(SOC) designs and ever changing compute, I/O trade offs requre revisiting these for newer implementations.
 
 In TCAMs we can use allthe entries for a lookup. That is utilization is 100%. However they are costly and power hungry. 
 For this reason, a full flow look up in a TCAM would be costly for an exact match lookup. To avoid digressing, we will not discuss how TCAMs are 
 optimal for a policy lookup here. 
 TCAMs support multiple key widths. Lower key width modes support more number of entries. For example, a 72 bit lookup doubles the number of entries 
 compared to a 144 bit lookup. To reduce the key width, and increse the number of entries, flows are usually hashed to a smaller value which is then 
 looked up in the TCAM for a match. This technique still has some small chance of hash collisions, but the probability is greatly reduced. 
 Larger the width of the hashed value, the better it is to avoid collisions.
 
 ## References
 
[TCAM based Flow Table - US Patent 7,197,597 ](https://patents.google.com/patent/US7197597B1)
