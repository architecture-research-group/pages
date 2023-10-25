

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
XFM sensitivity analyses can be reproduced using scripts compatible with the CM reproducibility project. The results include compression ratios 
achievable by XFM for different memory configurations, and an evaluation of contention between SPEC workloads and corunning (de)compression tasks.

Evaluation instructions:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Check-List (eta-information):
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* **Algorithm:** Model of the XFM Memory Module and Sensitivity Analyses
* **Program:**  SPEC 2017, Licensed Benchmark Suite
* **Compilation:** gcc 7.5.0 (Ubuntu 7.5.0-3ubuntu1~18.04)  
* **Run-time environment:** Tested on Ubuntu 18.04
* **Hardware:** Intel c6250 Server
* **Output:** Numerical results and plots corresponding to Figures 8, 11, and 12 in the paper.
* **Experiments:** As described in  Section 6 (Multi-Channel Mode) and Section 8 (Evaluation).
* **How much time is needed to prepare workflow (approximately)?:** Less than 10 minutes. Building SPEC 2017 is the most time consuming part.}
* **How much time is needed to complete experiments (approximately)?:** Less than 1 hour for XFM performance profiling tests (figures 8 and 12). Between 2-3 hours to regenerate SPEC (de)compression contention results from figure 11.}
* **Publicly available?:**  https://github.com/architecture-research-group/XFM-Artifacts.git
* **Code licenses (if publicly available)?:** MIT
* **Archived (provide DOI)?:** https://doi.org/10.5281/zenodo.8353767



Hardware dependencies
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Some experiments depend on access to a server which can be a multi-core server which can be allocated as a single Intel server (c6220 node) available through cloudlab. This hardware is sufficient to reproduce all results.


Installation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Obtain the code and sub-modules from github:

.. code-block:: bash

    git clone https://github.com/neel-patel-1/XFM_MICRO2023.git
    git submodule update --init --recursive

    
Instructions for each experiment are provided in each subdirectory of the repository.
Also, scripts for running each experiment are provided as bash shell scripts.



Publications
^^^^^^^^^^^^^^^^^^^
• Neel Patel, Amin Mamandipoor, Derrick Quinn, Mohammad Alian, "XFM: Accelerated Software-Defined Far Memory," MICRO 2023 [paper_] [slides_]

.. _paper: ?

.. _slides: ?


Personnel
^^^^^^^^^^^^^

• Neel_ Patel (Lead Author Student)

• Amin_ Mamandipoor (Co-Author Student) 

• Derrick_ Quinn (Co-Author Student) 

• Mohammad_ Alian (Principal Investigator)



.. _Neel: https://people.eecs.ku.edu/~n869p538/

.. _Amin: https://amin-mamandi.github.io/

.. _Derrick: https://www.linkedin.com/in/derrick-quinn-2427b717b/

.. _Mohammad: https://alian-eecs.ku.edu/

