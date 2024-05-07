.. this will make a link in the index.html
Userspace Networking in gem5
==================

Overview
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
In this work, we first demonstrate the limitations of gem5’s current network stack in achieving high network bandwidth. Then, we enable a userspace networking stack on gem5. We extend gem5’s NIC hardware model and device driver to support userspace device drivers running the DPDK framework. Additionally, we implement a network load generator hardware model in gem5 to generate various traffic patterns and perform per-packet timestamp and latency measurements without introducing packet loss. We develop a suite of six networkintensive benchmarks for stress testing the host network stack. These applications, based on DPDK, can run on both gem5 and real systems. Our experimental results show that enabling userspace networking improves gem5’s network bandwidth by 6.3× compared with the current Linux kernel software stack. We characterize the performance of DPDK benchmarks running on both a real system and gem5, and evaluate the sensitivity of the applications to various system and microarchitecture parameters. This work marks the first step in refactoring the networking subsystem in gem5.


Tutorials:
~~~~~~~~~~~~~~~
Please visit our open-source repository on github [here_] for detailed tutorial and links on how to enable userspace networking in gem5.

.. _here: https://github.com/architecture-research-group/gem5-dpdk-setup

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
