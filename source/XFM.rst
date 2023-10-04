.. title:: XFM
=======
Accelerating Software Defined Far Memory
==================

Overview
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
XFM (Accelerated Software Defined Far Memory) is a near-memory accelerated 
SFM architecture, which exploits
the coldness of data during SFM-initiated swap ins and outs. XFM
leverages refresh cycles to seamlessly switch the access control
of DRAM between the CPU and near-memory accelerator. XFM
parallelizes near-memory accelerator accesses with row refreshes
and removes the memory interference caused by SFM swap ins and
outs. 

Details
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Publications:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
• Neel Patel, Amin Mamandipoor, Derrick Quinn, Mohammad Alian, "XFM: Accelerated Software-Defined Far Memory" MICRO 2023 [paper_] [slides_]

Results:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
XFM sensitivity analyses can be reproduced using scripts compatible
with the CM reproducibility project. The results include compression ratios 
achievable by XFM for different memory configurations, and an evaluation of 
contention between SPEC workloads and corunning (de)compression tasks.

https://github.com/ctuning/cm-reproduce-research-projects/tree/main/script/reproduce-ieee-acm-micro2023-paper-28