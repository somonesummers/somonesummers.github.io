---
layout: page
title: Software	
---

This is some of the software that I wrote during my PhD.  It is a mix of a Python and C++.  

# MRI Software

## SLR Pulse Design

Shinnar-Le Roux (SLR) radiofrequency (RF) pulses are common on MRI.  While [http://rsl.stanford.edu/research/software.html many tools] exist to design such pulses, these are often in Matlab.  This is a version similar to the Pauly implementation, though the interface is written in Python and incorporates object-oriented design.  Source and examples are here.

[SLR design toolbox in Python](https://github.com/ekgibbons/slr)

The backend of this code is written in C\/C\+\+ and requires Boost 1.63 to compile (with the proper paths needing to be set in the Makefile).  C\+\+ code using the Boost Python\/Numpy interfaces.  This also requires an FIR pulse design toolbox I have designed.

## FIR Tools for Python

While Scipy has many FIR tools that are common in Matlab, it lacks a Type II FIR least squares filter design.  This toolbox includes this design.  This toolbox also include a Parks-McCellan FIR filter tool as well as method to transform a linear phase filter to a minimum phase filter.  This also requires Boost 1.63 to compile.  Source and examples are given below.

[FIR tools.](https://github.com/ekgibbons/fir FIR tools.)
