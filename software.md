---
layout: page
title: Software	
---

This is some of the software that I wrote during my PhD.  It is a mix of a Python and C++.

## SLR Pulse Design

Shinnar-Le Roux (SLR) radiofrequency (RF) pulses are common on MRI.  While [many tools](http://rsl.stanford.edu/research/software.html) exist to design such pulses, these are often in Matlab.  This is a version similar to the Pauly implementation, though the interface is written in Python and incorporates object-oriented design.  Source and examples are here.

[SLR design toolbox in Python](https://github.com/ekgibbons/slr)

The backend of this code is written in C/C++ and requires Boost 1.63 to compile (with the proper paths needing to be set in the Makefile).  C++ code using the Boost Python/Numpy interfaces.  This also requires an FIR pulse design toolbox I have designed.

## FIR Tools for Python

While Scipy has many FIR tools that are common in Matlab, it lacks a Type II FIR least squares filter design.  This toolbox includes this design.  This toolbox also include a Parks-McCellan FIR filter tool as well as method to transform a linear phase filter to a minimum phase filter.  This also requires Boost 1.63 to compile.  Source and examples are given below.

[FIR tools.](https://github.com/ekgibbons/fir)

## EPG Simulation Tools for Python

This is an implementation for performing extended phase graph simulations in Python.  This is heavily modeled after Brian Hargreaves' Matlab EPG simulation code.

[EPG python.](https://github.com/ekgibbons/epg-python)

## Bloch Simulation in C++

This software is primarily written in C++ with a Python interface.  This is a relatively fast brute force Bloch simulation.  This software allows the users to use custom RF pulses (SLR, VERSE, etc.) to accurately model rotations.  Thus, this simulation accounts for RF non-idealities such as slice profile.

Code is forthcoming (after I clean it up a bit for public use).