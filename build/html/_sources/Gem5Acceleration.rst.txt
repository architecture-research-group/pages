Software-based Architectural Simulation Acceleration
==================

Overviwe
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

gem5 is a cycle-accurate, open-source architectural simulator. It is widely used by reserchers both in academia and industry for a variety of purposes. But, it as slow! 
Our observations showed that gem5 runs up to 1.7x~3.7x faster on a MacBook Pro w/ M1 vs. Dell server w/ Intel Xeon Gold processor. 

We set out to find the answers to the following questions:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
• Where are the bottlenecks in a state-of-theart architectural simulator?
• How much faster can architectural simulations run by tuning system configurations?
• What are the opportunities in accelerating software simulation using hardware accelerators?

We started our investigation by looking at L1 cache size, as we suspecetd that it should be distinguishing factor between these two processors. 
Profiling gem5 on top of gem5 with different cache configuartions is super slow. FireSim was the solution!
FireSim is FPGA-accelerated hardware simulation platform. We are able to prove our assumptions about L1 cache size impact on the simulation's speedup by using FireSim. 

.. .. note:: note
.. .. warning:: warnings
.. .. tip:: tips
.. warning::  Runnig gem5 on FireSim is still slow.
.. .. image:: img/KU_campus_V2.png

.. ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. ----------------------------------------------------------------------------------

Evaluations Platforms
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This table summeries thye configurations of our three platforms.


+---------------------+----------------------+--------------------------------------------+--------------------------------------------+
| Platform            | Dell Precision 7920  | Apple Macbook                              | Apple MacStudio                            |
+=====================+======================+============================================+============================================+
| Config Name         | Intel_Xeon           | M1_Pro                                     | M1_Ultra                                   |
+---------------------+----------------------+--------------------------------------------+--------------------------------------------+
| SoC                 | Xeon Gold 6242R      | M1                                         |  M1 Ultra                                  |
+---------------------+----------------------+--------------------------------------------+--------------------------------------------+
| micro-architecture  | Cascade Lake         | Firestorm(P) + Icestorm(E)                 | Firestorm(P) + Icestorm(E)                 |
+---------------------+----------------------+--------------------------------------------+--------------------------------------------+
| Cores               | 20C/40T              |  P:4C/4T + E:4C/4T                         | P:16C/16T + E:4C/4T                        |
+---------------------+----------------------+--------------------------------------------+--------------------------------------------+
| Max Freq            | 3.1GHz (4.1GHz TB)   | 3.2GHz(P), 2GHz(E)                         | 3.2GHz(P), 2GHz(E)                         |
+---------------------+----------------------+--------------------------------------------+--------------------------------------------+
| L1 (per-core)       | 32KB(I) + 32KB(D)    | P:192KB(I) + 128KB(D) E:128KB(I) + 64KB(D) | P:192KB(I) + 128KB(D) E:128KB(I) + 64KB(D) |
+---------------------+----------------------+--------------------------------------------+--------------------------------------------+
|  L2                 | 20MB                 | P:12MB + E:4MB                             |  P:48MB + E:8MB                            |
+---------------------+----------------------+--------------------------------------------+--------------------------------------------+
| L3                  | 35.75MB              | 8MB                                       | 96MB                                        |
+---------------------+----------------------+--------------------------------------------+--------------------------------------------+
| Cacheline           | 64B                  | 128B                                       | 128B                                       |
+---------------------+----------------------+--------------------------------------------+--------------------------------------------+
| Memory              | 96GB, DDR4-2933      | 8GB, LPDDR4X-4266                          | 64GB, LPDDR5-6400                          |
+---------------------+----------------------+--------------------------------------------+--------------------------------------------+
| DRAM BW             | 141 GB/s             | 68 GB/s                                    | 68 GB/s                                    |
+---------------------+----------------------+--------------------------------------------+--------------------------------------------+
| VM page size        | 4KB                  | 16KB                                       | 16KB                                       |
+---------------------+----------------------+--------------------------------------------+--------------------------------------------+



CPU Types
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
We change the CPU type, number of CPUs, and memory size. We use the following CPU types:

**AtomicSimpleCPU (Atomic):** CPU type with CPI = 1 where memory accesses are atomic and completed without modeling any contention or queuing delays.

**TimingSimpleCPU (Timing):** CPU type with CPI = 1 where memory accesses are modeled in detail considering the queuing delays and resource contentions in the memory and interconnect.

**In-order CPU (Minor):** In-order or Minor CPU models a fixed pipeline with strict in-order instruction execution. Minor CPU uses the detailed timing memory mode  for accessing memory.

**Out-of-order CPU (O3):** O3 CPU models an out-of-order superscalar loosely based on the Alpha 2126 core. O3 CPU uses the detailed timing memory model for accessing memory.

.. tip::  Simple CPUs are used for fast-forwarding simulation, warming up caches, or for studies that do not require detailed CPU modeling. In-order and out-of-order CPU models are used for detailed micro-architectural studies.




Running gem5 on FireSim​
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

We should start with the FireSim CPU configuration:

Base Hardware Configuration on FireSim
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
=======================================     =========================================================================================================================================================================================================================================================================================================================  
Parameters										Value							
=======================================     =========================================================================================================================================================================================================================================================================================================================  
Core Frequency                                  4GHz
Number of Cores                                 4 Cores
Superscalar                                     8-width wide
ROB/IQ/LQ/SQ Entries                            192/64/32/32
Int & FP Registers                              128 & 192
Branch Predictor/BTB Entries                    TournamentBP/4096
Cache: L1I/L1D                                  48KB(I), 32KB(D)
DRAM                                            2GB, DDR3-1600-8x8
Operating System                                Linux Linaro (kernel 5.4.0)
=======================================     ========================================================================================================================================================================================================================================================================================================================= 

.. tip:: More information on FireSim website - https://fires.im/



How to run gem5 on FireSim
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Here are the steps to run gem5 on FireSim:

1. Set up the AWS FireSim environment

2. Build the gem5 binary for RISCV ISA

3. Prepare gem5 workload and transfer it to the instance

4. Create FireSim workload using FireMarshal

5. Build our target design

6. PModify parameters, tests, and results



We used a Z1d.2xlarge FireSim manager instance:

.. code-block:: bash

    mosh --ssh"=ssh -i firesim.pem" username@ip_addr

Change FireSim
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We specify a quad-core rocket chip with a 64KB L1 icache and dcache in the TargetConfigs.scala file. Parameter at the top of arrow overrides others below.


.. code-block:: yaml  

    class FireSimGem5ConfigQuadRocketConfig extends Config(
    new freechips.rocketchip.subsystem.WithL1ICacheWays(16) ++  // change rocket I$
    new freechips.rocketchip.subsystem.WithL1ICacheSets(64) ++ // change rocket I$
    new freechips.rocketchip.subsystem.WithL1DCacheWays(16) ++  // change rocket D$ 
    new freechips.rocketchip.subsystem.WithL1DCacheSets(64) ++ // change rocket D$
    new WithDefaultFireSimBridges ++
    new WithDefaultMemModel ++
    new WithFireSimConfigTweaks ++
    new chipyard.QuadRocketConfig)

We modified FireSim .json file to run gem5 

.. code-block:: json 

    {
    "benchmark_name": "gem5-workload",
    "common_simulation_outputs": [ "uartlog","memory_stats*.csv", "TRACEFILE*"],
    "common_simulation_inputs": ["gem5-workload-gem5-bin-dwarf"],
    "post_run_hook": "gen-all-flamegraphs-fireperf.sh",
    "workloads": [ {
    "name": "gem5-workload-gem5",
    "bootbinary": "../../../target-design/chipyard/software/firemarshal/images/gem5-workload-gem5-bin",
    "rootfs": "../../../target-design/chipyard/software/firemarshal/images/gem5-workload-gem5.img",
    "outputs": [ "/root/sim-environment/m5out" ] } ]
    }


We used a quad-core Rocket Chip with an 8KB 2-way set associative icache & dcache, and a 512KB l2 cache base config. Also, we modified *config_build.yaml*, *config_hwdb.yaml*, *config_runtime.yaml*, and *config_build_receipes.yaml* files:


.. code-block:: bash

    /home/centos/firesim/target-design/chipyard/generators/firechip/src/main/scala/TargetConfigs.Scala
    /home/centos/firesim/target-design/chipyard/generators/chipyard/src/main/scala/config/RocketConfigs.scala    


.. warning:: To change the base system configuration, we had to specify new design parameters in TargetConfigs.scala or RocketConfigs.scala

Next, we use golden gate compiler to generate the verilog code from the Chisel-generated RTL code for the AWS AGFI build process.

FireSim requires a *.json* input file format to define workloads (e.g. gem5) that will run on the target design. FireMarshal is used to manage this process.

.. tip::  Check out the FireMarshal documentation for more details. https://firemarshal.readthedocs.io/en/latest/index.html



Modifying config_build_recipe.yaml

.. code-block:: yaml

    firesim_rocket_quadcore_gem5_config: // This can be any name specified by the user
    DESIGN: FireSim
    TARGET_CONFIG: DDR3FRFCFSLLC4MB_WithDefaultFireSimBridges_WithFireSimTestChipConfigTweaks_FireSimGem5Config19QuadRocketConfig
    PLATFORM_CONFIG: WithAutoILA_F140MHz_BaseF1Config
    deploy_triplet: null
    post_build_hook: null
    metasim_customruntimeconfig: null
    bit_builder_recipe: bit-builder-recipes/f1.yaml


We have modified  config_build.yaml like below:

.. code-block:: yaml

    builds_to_run:
    firesim_rocket_quadcore_gem5_config  // This name must match the name specified in config_build_recipes.yam

**Modifying config_runtime.yaml**

.. code-block:: yaml

    run_farm:
    run_farm_hosts_to_use:
    - f1.4xlarge: 1
    Tracing:
    enable: yes       # optional​
    output_format: 2         # flame graph​
    # Trigger selector.​
    selector: 3              # Instruction Trigger within the target software like gem5_workload.json​
    start: ffffffff00008013
    end: ffffffff00010013
    workload:
    workload_name: gem5-workload.json

**Modifying the gem5 runscript**

.. code-block:: bash

    firesim-start-trigger && ./gem5_script.sh && firesim-end-trigger # run in the bash prompt of the simulated system​


**Golden gate**

.. code-block:: bash

   Golden gate compiler lives here at /home/centos/firesim/sim/

**Make**

.. code-block:: bash

   make DESIGN=FireSim TARGET_CONFIG=DDR3FRFCFSLLC4MB_WithDefaultFireSimBridges_WithFireSimTest ChipConfigTweaks _FireSimGem5ConfigQuadRocketConfig PLATFORM_CONFIG=WithAutoILA_F140MHz_ BaseF1Config f1

**buildbitstream**

.. code-block:: bash

    This launches the buildfarm instance, calls the golden gate compiler with the parameters specified in config_build_recipes.yaml and produces an AGFI that runs on the FPGA.


**Updating config_hwdb.yaml**

.. code-block:: yaml

    firesim_rocket_quadcore_gem5_config4
    agfi: agfi-0ae1574040e7ff0fc
    deploy_triplet_override: null
    custom_runtime_config: null
    firesim_rocket_quadcore_gem5_config5: # Add your AGFI info to config_hwdb.yaml, so they can be deployed during simulation​
    agfi: agfi-06e876ba9378cc9ff
    deploy_triplet_override: null
    custom_runtime_config: null
    firesim_rocket_quadcore_gem5_config6:
    agfi: agfi-00a966236eb672af3
    deploy_triplet_override: null
    custom_runtime_config: null
    firesim_rocket_quadcore_gem5_config7:
    agfi: agfi-0bc64b009db5f849a
    deploy_triplet_override: null
    custom_runtime_config: null

**Run gem5 on FireSim**

.. code-block:: bash

    firesim launchrunfarm; firesim infrasetup; firesim runworkload


.. tip:: This launches the runfarm instance, sets up the simulation infrastructure, and begins simulation of the gem5 workload on the specified Target Design.


.. tip:: gem5 and FireSim stats can be read from /home/centos/firesim/results-workload/




.. if you need hyperlink, you can use this template: 

.. firesim website is this_

.. .. _this: https://fires.im/


