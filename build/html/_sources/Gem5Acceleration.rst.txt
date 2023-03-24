.. this will make a link in the index.html
Gem 5 Acceleration
==================

Overview
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
This is just a simple copy-paste from the paper. We need to trim it...

Software-based simulation is the backbone of computer architecture research and development. Since the inception of computer architecture as a field, many software-based
architectural simulators1 have emerged. Currently, various architectural simulators are in-use by academia and industry for modeling different aspects of future computing platforms.
gem5, Sniper, MARSSx86, and ZSim are just a few examples of architectural simulators with currently active communities. gem5 is one of the most popular hardware simulator in the community. Howerver,
software-based architectural simulation is slow. We observed that gem5 runs up to 1.7x~3.7x faster on a MacBook Pro w/ M1 vs. Dell server w/ Intel Xeon Gold.
So, we extensively profiled gem5 using hardware performance counters on both Intel and Apple's CPUs. We found that gem5 is fronend-bounded and it is very sensetive to L1 cache size.


.. calculating Velocity Feed Forward gain (kF)
.. ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. the "tilde" underline will greate a sub-sub section with a link 


.. .. this will make a smaller bold template
.. Do I need to calculate kF?
.. ----------------------------------------------------------------------------------
.. If using any of the control modes, we recommend calculating the kF:


.. this is how you can make a waring
..warning:: That's how we should include warnings
.. this is how you can make a note
..note:: That's how we should include notes
.. this is how you can make tips
..tip:: That's how you should include tips
.. this is how you insert an image, make sure it is alse in the img folder
..image:: img/KU_campus_V2.png



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


Configurations
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Add some text ....
We change the CPU type, number of CPUs, and memory size. We use the following CPU types:

AtomicSimpleCPU (Atomic)
----------------------------------------------------------------------------------
CPU type with CPI = 1 where memory accesses are atomic and completed without modeling any contention or queuing delays.

TimingSimpleCPU (Timing)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
CPU type with CPI = 1 where memory accesses are modeled in detail considering the queuing delays and resource contentions in the memory and interconnect.

In-order CPU (Minor)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
In-order or Minor CPU models a fixed pipeline with strict in-order instruction execution. Minor CPU uses the detailed timing memory mode  for accessing memory.

Out-of-order CPU (O3)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
O3 CPU models an out-of-order superscalar loosely based on the Alpha 2126 core. O3 CPU uses the detailed timing memory model for accessing memory.

Some text refering to the table below ....

.. heres how to put in a table with scrolling
Base Hardware Configuration on FireSim
----------------------------------------------------------------------------------
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



We set out to find the answers to the following questions 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
• Where are the bottlenecks in a state-of-theart architectural simulator?
•  How much faster can architectural simulations run by tuning system configurations?
• What are the opportunities in accelerating software simulation using hardware accelerators?


FireSim HowTo
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

some text talking about the firesim and refering to the steps below.
also refer to the firesim website for more information - https://fires.im/

How to run gem5 on FireSim
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Set up the AWS FireSim environment

2. Build the gem5 binary for RISCV ISA

3. Prepare gem5 workload and transfer it to the instance

4. Create FireSim workload using FireMarshal

5. Build our target design

6. PModify parameters, tests, and results


Change FireSim
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
We specify a quad-core rocket chip with a 64KB L1 icache and dcache in the TargetConfigs.scala file. Parameter at the top of arrow overrides others below.

.. code-block:: bash
    lass FireSimGem5ConfigQuadRocketConfig extends Config(

    new freechips.rocketchip.subsystem.WithL1ICacheWays(16) ++  // change rocket I$

    new freechips.rocketchip.subsystem.WithL1ICacheSets(64) ++ // change rocket I$

    new freechips.rocketchip.subsystem.WithL1DCacheWays(16) ++  // change rocket D$ 

    new freechips.rocketchip.subsystem.WithL1DCacheSets(64) ++ // change rocket D$

    new WithDefaultFireSimBridges ++

    new WithDefaultMemModel ++

    new WithFireSimConfigTweaks ++

    new chipyard.QuadRocketConfig)


Change FireSim
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
We specify a quad-core rocket chip with a 64KB L1 icache and dcache in the TargetConfigs.scala file. Parameter at the top of arrow overrides others below.

.. code-block:: bash

    mosh --ssh"=ssh -i firesim.pem" username@ip_addr


gem5 as a Workload on FireSim
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
FireSim .json file is modified to run gem5 

.. code-block:: bash 

    ,json file should be added!

Building our target design
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We use a quad-core Rocket Chip with an 8KB 2-way set associative icache & dcache, and a 512KB l2 cache base config. 

We modify config_build.yaml, config_hwdb.yaml, config_runtime.yaml, & config_build_receipes.yaml files

.. code-block:: bash
    /home/centos/firesim/target-design/chipyard/generators/firechip/src/main/scala/TargetConfigs.Scala
    /home/centos/firesim/target-design/chipyard/generators/chipyard/src/main/scala/config/RocketConfigs.scal    


To change the base system configuration, we had to specify new design parameters in TargetConfigs.scala or RocketConfigs.scala

Next, we use golden gate compiler to generate the verilog code from the Chisel-generated RTL code for the AWS AGFI build process.

gem5 as a Workload on FireSim
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~





FireSim requires a .json input file format to define workloads (e.g. gem5) that will run on the target design. FireMarshal is used to manage this process. Check out the FireMarshal documentation for more details.

    - https://firemarshal.readthedocs.io/en/latest/index.html


gem5 as a Workload on FireSim
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash 
    

    "benchmark_name": "gem5-workload",
    "common_simulation_outputs": [ "uartlog","memory_stats*.csv", "TRACEFILE*"],
    "common_simulation_inputs": ["gem5-workload-gem5-bin-dwarf"],
    "post_run_hook": "gen-all-flamegraphs-fireperf.sh",
    "workloads": [ {
    "name": "gem5-workload-gem5",
    "bootbinary": "../../../target-design/chipyard/software/firemarshal/images/gem5-workload-gem5-bin",
    "rootfs": "../../../target-design/chipyard/software/firemarshal/images/gem5-workload-gem5.img",
    "outputs": [ "/root/sim-environment/m5out" ] } ]



Modifying the config scripts
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash
    Modifying config_build_recipe.yaml
    firesim_rocket_quadcore_gem5_config: // This can be any name specified by the user
    DESIGN: FireSim
    TARGET_CONFIG: DDR3FRFCFSLLC4MB_WithDefaultFireSimBridges_WithFireSimTestChipConfigTweaks_FireSimGem5Config19QuadRocketConfig
    PLATFORM_CONFIG: WithAutoILA_F140MHz_BaseF1Config
    deploy_triplet: null
    post_build_hook: null
    metasim_customruntimeconfig: null
    bit_builder_recipe: bit-builder-recipes/f1.yaml


Modifying config_build.yaml

.. code-block:: bash
    builds_to_run:​

    firesim_rocket_quadcore_gem5_config  // This name must match the name specified in config_build_recipes.yam



We used a Z1d.2xlarge FireSim manager instance

.. code-block:: bash

    //add the commandline code here, for example how to ssh into the f1 instance
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
}

if you need hyperlink, you can use this template: 

firesim website is this_

.. _this: https://fires.im/


