.. this will make a link in the index.html
Gem 5 Acceleration
==================

.. yes there are alot of robtics refrenaces in here....

.. this will make a bold heading and make a link in the navigation bar under the project

code
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. this is an example of how to do a code sinp-it mace sure you specify the language for code highlighting
.. code-block:: java

    //this is how you add code


.. this is how to make a bold heading as a sub link under the code section
calculating Velocity Feed Forward gain (kF)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
the "tilde" underline will greate a sub-sub section with a link 


.. this will make a smaller bold template
Do I need to calculate kF?
----------------------------------------------------------------------------------
If using any of the control modes, we recommend calculating the kF:


.. this is how you can make a waring
.. warning:: Arbitrary Feed Forward and Auxiliary Closed Loop cannot be used simultaneously *except* when using Motion Profile Arc.
.. this is how you can make a note
.. note:: The output of the PIDF controller in Talon/Victor uses 1023 as the “full output".
.. this is how you can make tips
.. tip:: If your elevator mechanism will change weight while in use (i.e. pick up a heavy game piece), it is helpful to measure gravity offsets at each expected weight and switch between Arbitrary Feed Forward values as needed.
.. this is how you insert an image, make sure it is alse in the img folder
.. image:: img/KU_campus_V2.png



.. this is how you can make a table
General Closed-Loop Configs
----------------------------------------------------------------------------------
+----------------------------------------+------------------------------------------------------------------------+
|                Name                    |                         Description                                    |
+----------------------------------------+------------------------------------------------------------------------+
| PID 0 Primary Feedback Sensor          |  | Selects the sensor source for PID0 closed loop, soft limits, and    |
|                                        |  | value reporting for the SelectedSensor API.                         |
+----------------------------------------+------------------------------------------------------------------------+
| PID 0 Primary Sensor Coefficient       |  | Scalar (0,1] to multiply selected sensor value before using.        |
|                                        |  | Note this will reduce resolution of the closed-loop.                |
+----------------------------------------+------------------------------------------------------------------------+
| PID 1 Aux Feedback Sensor              |  Select the sensor to use for Aux PID[1].                              |
+----------------------------------------+------------------------------------------------------------------------+
| PID 1 Aux Sensor Coefficient           |  | Scalar (0,1] to multiply selected sensor value before using.        |
|                                        |  | Note that this will reduce the resolution of the closed-loop.       |
+----------------------------------------+------------------------------------------------------------------------+
| PID 1 Polarity                         |  | False: motor output = PID[0] + PID[1],  follower = PID[0] - PID[1]. |
|                                        |  | True : motor output = PID[0] - PID[1],  follower = PID[0] + PID[1]. |
|                                        |  | This only occurs if follower is an auxiliary type.                  |
+----------------------------------------+------------------------------------------------------------------------+
| Closed Loop Ramp                       |  | How much ramping to apply in seconds from neutral-to-full.          |
|                                        |  | A value of 0.100 means 100ms from neutral to full output.           |
|                                        |  | Set to 0 to disable.                                                |
|                                        |  | Max value is 10 seconds.                                            |
+----------------------------------------+------------------------------------------------------------------------+



.. heres how to put in a table with scrolling
Motion Magic Closed-Loop Configs
----------------------------------------------------------------------------------
=======================================     =========================================================================================================================================================================================================================================================================================================================  
Name										Description							
=======================================     =========================================================================================================================================================================================================================================================================================================================  
Acceleration								Motion Magic target acceleration in (sensor units per 100ms) per second.
Cruise Velocity                   			Motion Magic maximum target velocity in sensor units per 100ms.
S-Curve Strength                   			Zero to use trapezoidal motion during motion magic.  [1,8] for S-Curve, higher value for greater smoothing.
=======================================     ========================================================================================================================================================================================================================================================================================================================= 



.. this is how you can make a bulleted list
Before invoking any of the closed loop modes, the following must be done:

• Complete the sensor bring up procedure to ensure sensor phase and general health.
• Record the maximum sensor velocity (position units per 100ms) at 100% motor output.
• Calculate an Arbitrary Feed Forward if necessary (gravity compensation, custom system characterization).
• Calculating Velocity Feed-Forward (kF) gain if applicable (Velocity Closed Loop, Motion Profile, Motion Magic).


.. this is how toy can make a numbered list
1. Checkout the relevant example from CTREs GitHub.

2. Set all of your gains to zero.  Use either API or Phoenix Tuner.

3. If not using Position-Closed loop mode, set the kF to your calculated value (see previous section).

4. If using Motion Magic, set your initial cruise velocity and acceleration (section below).

5. Deploy the application and use the joystick to adjust your target.  Normally this requires holding down a button on the gamepad (to enter closed loop mode).

6. Plot the sensor-position to assess how well it is tracking.  This can be done with WPI plotting features, or with Phoenix Tuner.


.. this is how you put in a url
- https://github.com/CrossTheRoadElec/Phoenix-Examples-Languages


.. this is how you put in a hyperlink
For scaling the value, the cosine term of trigonometry_ matches the scaling we need for our rotating arm.  The cosine term is at maximum value (+1) when at horizontal (0 degrees or radians) and is at 0 when the arm is vertical (90 degrees or pi/2 radians).
To use this cosine value as a scalar, we will need to determine our current angle.  This requires knowing the current arm position and number of position ticks per degree, then converting to units of radians.

.. _trigonometry: https://en.wikipedia.org/wiki/Trigonometry

