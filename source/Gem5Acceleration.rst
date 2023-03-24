.. this will make a link in the index.html
Gem 5 Acceleration
==================

Overviwe
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
This is just a simple copy-paste from the paper. We need to trim it...

Software-based simulation is the backbone of computer architecture research and development. Since the inception of computer architecture as a field, many software-based
architectural simulators1 have emerged. Currently, various architectural simulators are in-use by academia and industry for modeling different aspects of future computing platforms.
gem5, Sniper, MARSSx86, and ZSim are just a few examples of architectural simulators with currently active communities. gem5 is one of the most popular hardware simulator in the community. Howerver,
software-based architectural simulation is slow. We observed that gem5 runs up to 1.7x~3.7x faster on a MacBook Pro w/ M1 vs. Dell server w/ Intel Xeon Gold.
So, we extensively profiled gem5 using hardware performance counters on both Intel and Apple's CPUs. We found that gem5 is fronend-bounded and it is very sensetive to L1 cache size.

FireSim commandline
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. this is an example of how to do a code sinp-it mace sure you specify the language for code highlighting
.. code-block:: shell

    //add the commandline code here, for example how to ssh into the f1 instance



.. calculating Velocity Feed Forward gain (kF)
.. ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. the "tilde" underline will greate a sub-sub section with a link 


.. .. this will make a smaller bold template
.. Do I need to calculate kF?
.. ----------------------------------------------------------------------------------
.. If using any of the control modes, we recommend calculating the kF:


.. this is how you can make a waring
.. warning:: Arbitrary Feed Forward and Auxiliary Closed Loop cannot be used simultaneously *except* when using Motion Profile Arc.
.. this is how you can make a note
.. note:: The output of the PIDF controller in Talon/Victor uses 1023 as the “full output".
.. this is how you can make tips
.. tip:: If your elevator mechanism will change weight while in use (i.e. pick up a heavy game piece), it is helpful to measure gravity offsets at each expected weight and switch between Arbitrary Feed Forward values as needed.
.. this is how you insert an image, make sure it is alse in the img folder
.. image:: img/KU_campus_V2.png



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
• Where are the bottlenecks in a state-of-theart architectural simulator?
•  How much faster can architectural simulations run by tuning system configurations?
• What are the opportunities in accelerating software simulation using hardware accelerators?



How to run gem5 on FireSim
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Set up the AWS FireSim environment

2. Build the gem5 binary for RISCV ISA

3. Prepare gem5 workload and transfer it to the instance

4. Create FireSim workload using FireMarshal

5. Build our target design

6. PModify parameters, tests, and results



.. this is how you put in a url
Here is the link to FireSim website for more information
- https://fires.im/


.. this is how you put in a hyperlink
.. For scaling the value, the cosine term of trigonometry_ matches the scaling we need for our rotating arm.  The cosine term is at maximum value (+1) when at horizontal (0 degrees or radians) and is at 0 when the arm is vertical (90 degrees or pi/2 radians).
.. To use this cosine value as a scalar, we will need to determine our current angle.  This requires knowing the current arm position and number of position ticks per degree, then converting to units of radians.

.. .. _trigonometry: https://en.wikipedia.org/wiki/Trigonometry

