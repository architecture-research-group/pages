.. this will make a link in the index.html
Userspace Networking in gem5
==================

Overview
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
In this work, we first demonstrate the limitations of gem5’s current network stack in achieving high network bandwidth. Then, we enable a userspace networking stack on gem5. We extend gem5’s NIC hardware model and device driver to support userspace device drivers running the DPDK framework. Additionally, we implement a network load generator hardware model in gem5 to generate various traffic patterns and perform per-packet timestamp and latency measurements without introducing packet loss. We develop a suite of six networkintensive benchmarks for stress testing the host network stack. These applications, based on DPDK, can run on both gem5 and real systems. Our experimental results show that enabling userspace networking improves gem5’s network bandwidth by 6.3× compared with the current Linux kernel software stack. We characterize the performance of DPDK benchmarks running on both a real system and gem5, and evaluate the sensitivity of the applications to various system and microarchitecture parameters. This work marks the first step in refactoring the networking subsystem in gem5.


.. calculating Velocity Feed Forward gain (kF)
.. ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. the "tilde" underline will greate a sub-sub section with a link 


.. .. this will make a smaller bold template
.. Do I need to calculate kF?
.. ----------------------------------------------------------------------------------
.. If using any of the control modes, we recommend calculating the kF:




.. .. this is how you can make a table
.. General Closed-Loop Configs
.. ----------------------------------------------------------------------------------
.. +----------------------------------------+------------------------------------------------------------------------+
.. |               Parameters                |                         Description                                    |
.. +----------------------------------------+------------------------------------------------------------------------+
.. | PID 0 Primary Feedback Sensor          |  | Selects the sensor source for PID0 closed loop, soft limits, and    |
.. |                                        |  | value reporting for the SelectedSensor API.                         |
.. +----------------------------------------+------------------------------------------------------------------------+
.. | PID 0 Primary Sensor Coefficient       |  | Scalar (0,1] to multiply selected sensor value before using.        |
.. |                                        |  | Note this will reduce resolution of the closed-loop.                |
.. +----------------------------------------+------------------------------------------------------------------------+
.. | PID 1 Aux Feedback Sensor              |  Select the sensor to use for Aux PID[1].                              |
.. +----------------------------------------+------------------------------------------------------------------------+
.. | PID 1 Aux Sensor Coefficient           |  | Scalar (0,1] to multiply selected sensor value before using.        |
.. |                                        |  | Note that this will reduce the resolution of the closed-loop.       |
.. +----------------------------------------+------------------------------------------------------------------------+
.. | PID 1 Polarity                         |  | False: motor output = PID[0] + PID[1],  follower = PID[0] - PID[1]. |
.. |                                        |  | True : motor output = PID[0] - PID[1],  follower = PID[0] + PID[1]. |
.. |                                        |  | This only occurs if follower is an auxiliary type.                  |
.. +----------------------------------------+------------------------------------------------------------------------+
.. | Closed Loop Ramp                       |  | How much ramping to apply in seconds from neutral-to-full.          |
.. |                                        |  | A value of 0.100 means 100ms from neutral to full output.           |
.. |                                        |  | Set to 0 to disable.                                                |
.. |                                        |  | Max value is 10 seconds.                                            |
.. +----------------------------------------+------------------------------------------------------------------------+


.. Configurations
.. ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. Add some text ....
.. We change the CPU type, number of CPUs, and memory size. We use the following CPU types:

.. AtomicSimpleCPU (Atomic)
.. ----------------------------------------------------------------------------------
.. CPU type with CPI = 1 where memory accesses are atomic and completed without modeling any contention or queuing delays.

.. TimingSimpleCPU (Timing)
.. ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. CPU type with CPI = 1 where memory accesses are modeled in detail considering the queuing delays and resource contentions in the memory and interconnect.

.. In-order CPU (Minor)
.. ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. In-order or Minor CPU models a fixed pipeline with strict in-order instruction execution. Minor CPU uses the detailed timing memory mode  for accessing memory.

.. Out-of-order CPU (O3)
.. ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. O3 CPU models an out-of-order superscalar loosely based on the Alpha 2126 core. O3 CPU uses the detailed timing memory model for accessing memory.

.. Some text refering to the table below ....

.. .. heres how to put in a table with scrolling
.. Base Hardware Configuration on FireSim
.. ----------------------------------------------------------------------------------
.. =======================================     =========================================================================================================================================================================================================================================================================================================================  
.. Parameters										Value							
.. =======================================     =========================================================================================================================================================================================================================================================================================================================  
.. Core Frequency                                  4GHz
.. Number of Cores                                 4 Cores
.. Superscalar                                     8-width wide
.. ROB/IQ/LQ/SQ Entries                            192/64/32/32
.. Int & FP Registers                              128 & 192
.. Branch Predictor/BTB Entries                    TournamentBP/4096
.. Cache: L1I/L1D                                  48KB(I), 32KB(D)
.. DRAM                                            2GB, DDR3-1600-8x8
.. Operating System                                Linux Linaro (kernel 5.4.0)
.. =======================================     ========================================================================================================================================================================================================================================================================================================================= 

Publications
^^^^^^^^^^^^^^^^^^^
• Johnson Umeike, Siddharth Agarwal, Nikita Lazarev, Mohammad Alian, "Userspace Networking in gem5," ISPASS 2023 [paper_] [slides_]

.. _paper: https://kansas-my.sharepoint.com/:b:/g/personal/m258a886_home_ku_edu/EerhTJA-ylBGmTJBkRRSuHwBi8j4ejPcSLKhBsMvltj9zA?e=JYmhV0

.. _slides: https://kansas-my.sharepoint.com/:p:/g/personal/c834u979_home_ku_edu/EUMSrOwoR7pPoHCWeSccFw0BSWumV2HBLMdwVAC8e8cNTQ?e=2W8hRj


Personnel
^^^^^^^^^^^^^

• Johnson_ Umeike (Lead Author Student)

• Siddharth_ Agarwal (Co-Author Student)

• Nikita_ Lazarev (Co-Author Student) 

• Mohammad_ Alian (Principal Investigator)

.. _Johnson: https://jumeike.github.io/

.. _Siddharth: https://www.linkedin.com/in/siddharth-agarwal99/

.. _Nikita: https://www.nikita.tech/

.. _Mohammad: https://alian-eecs.ku.edu/
