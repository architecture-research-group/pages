

SmartDIMM: In-Memory Acceleration of Upper Layer Protocols
======================================================

Overview
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
There has been significant focus on offloading upper-
layer network protocols (ULPs) to accelerators located on CPUs
and SmartNICs. However, restricting accelerator placement to
these locations limits both the variety of ULPs that can be acceler-
ated and the overall performance. In particular, it overlooks the
opportunity to accelerate ULPs running atop a stateful transport
protocol in the face of high cache contention. That is, at high
network rates, the frequent DRAM accesses and SmartNIC-CPU
synchronizations outweigh the benefits of hardware acceleration.
This work introduces SmartDIMM, which unlocks the opportunity
for accelerating ULPs running atop stateful transport protocols
that primarily operate on data stored in DRAM. We prototyped
SmartDIMM using Samsung’s AxDIMM and implemented end-
to-end offloading of (de/en)cryption and (de)compression– two
ULPs widely employed in datacenters. We then compared the
performance of SmartDIMM with accelerator placements on the
CPU, SmartNIC, and PCIe cards. Our results demonstrate that
ULP offloading on SmartDIMM outperforms CPU, SmartNIC
and PCIe-based offload configurations. In comparison to a
server executing (de/en)cryption and (de)compression on the
CPU, SmartDIMM achieves 21.0% to 10.28× higher requests per
second and 36.3% to 88.9% lower memory bandwidth utilization.


Check-List (meta-information):
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* **Algorithm:** SmartDIMM
* **Program:**  Nginx, SmartDIMM-compatible nginx compression module (source provided)
* **Compilation:** gcc 8.4.0 (Ubuntu 8.4.0-3ubuntu2) 
* **Run-time environment:** Tested on Ubuntu 20.04
* **Hardware:** Intel c6250 Server
* **Output:** Numerical results and plots corresponding to Figures 8, 11, and 12 in the paper.
* **Experiments:** As described in Sec. VII-B
* **How much time is needed to prepare workflow (approximately)?:** Less than 10 minutes. Building SPEC 2017 is the most time consuming part.}
* **How much time is needed to complete experiments (approximately)?:** Less than 1 hour for XFM performance profiling tests (figures 8 and 12). Between 2-3 hours to regenerate SPEC (de)compression contention results from figure 11.}
* **Publicly available?:**  https://github.com/architecture-research-group/SmartDIMM_ArtifactEvaluation.git
* **Code licenses (if publicly available)?:** MIT
* **Archived (provide DOI)?:** https://doi.org/10.5281/zenodo.10278844



Hardware dependencies
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Experiments depend on access to cloudlab servers:

* Allocate a cloudlab instance using the genilib script provided in this repo
* Create a cloudlab account if needed
* Navigate to Experiments
* Then Create Experiment Profile,
* Upload nginx_workload.profile


Installation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Obtain the code and sub-modules from github:

.. code-block:: bash
    # on DUT
    git clone git@github.com:architecture-research-group/SmartDIMM_ArtifactEvaluation.git
    git submodule update --init --recursive
    cd wrk_offloadenginesupport/async_nginx_build
    make all

    # on Workload Generator
    git clone git@github.com:architecture-research-group/SmartDIMM_ArtifactEvaluation.git
    git submodule update --init --recursive
    cd wrk_offloadenginesupport
    make all
    
Instructions for each experiment are provided in each subdirectory of the repository.
Also, scripts for running each experiment are provided as bash shell scripts.

Publications
^^^^^^^^^^^^^^^^^^^
• Neel Patel, Amin Mamandipoor, Mohammad Nouri, Mohammad Alian, "SmartDIMM: In-Memory Acceleration of Upper Layer Protocols" HPCA 2024 [paper_] [slides_]

.. _paper: ?

.. _slides: ?


Personnel
^^^^^^^^^^^^^

• Neel_ Patel (Lead Co-Author Student)

• Amin_ Mamandipoor (Lead Co-Author Student) 

• Mohammad_ Nouri (Co-Author Student) 

• Mohammad__ Alian (Principal Investigator)



.. _Neel: https://people.eecs.ku.edu/~n869p538/

.. _Amin: https://amin-mamandi.github.io/

.. _Mohammad: https://arg.ku.edu/build/html/staff.html

.. __Mohammad: https://alian-eecs.ku.edu/

