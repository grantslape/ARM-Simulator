=======================
Lab 3: An ARM Simulator
=======================

This overview written by Grant H. Slape.

Overview
--------

This project was forked from https://github.com/mansigoel/ARM-Simulator and originally developed by Mansi Goel and Taneea S Agrawaal.
This program simulates a small subset of the ARM instruction set.

Usage
~~~~~

From the src directory to compile::

	$ make

To execute from main directory (test files can be found in (bin)::

	$ bin/myARMSim <INPUT.MEM>

Input memory file must be passed in as an argument.

Functionality
-------------

Background
~~~~~~~~~~~

This simulator follows the same basic process as all processor, fetch, decode, execute, retire.
In this project we do end up performing actions during the retire step - namely writeback.
It goes without saying that when using the ALU we need to writeback to results of our operation!
Debug messages will be printed for each step of the program during exection.  The machine components themselves are global static variables inside myARMSim.c and are not visible to outside interference.

Execution Specifics
~~~~~~~~~~~~~~~~~~~

This program came with some example code for testing the simulator.  quoting the author documentation::

	We test the simulator with following assembly programs:
	-	Fibonacci Program
	-	Sum of the array of N elements. Initialize an array in first loop with each element equal to its index. In second loop find the sum of this array, and store the result at Arr[N].   

Miscellanous
~~~~~~~~~~~~

The original documentation for this project can be found in the /doc/ directory, in .docx format.  I've converted it to RST and renamed it readme.rst.
The documentation assists in understanding the ISA for this processor, and how the decode step works.
There is also a good explanation for how branch addresses are calculated.