# Overview
This Python-based resistance feedback control system provides automated voltage regulation for single or multi-channel operation across microheater arrays. <br> 
The system continuously monitors resistances and adjusts voltages to maintain target resistances within a tolerance range, with built-in fail-safe mechanisms and the ability to run multiple feedback sequences. 

# System Architecture
The control system consists of two main functions that work together:

## 1. feedback_loop()
Monitors microheater resistances and adjsuts voltages to stabilise microheaters at defined target resistances through continuous feedback control.

## 2. series_feedback()
Executes multiple feedback loops in a sequential order, with each feedback loop exiting after a specified hold_duration <br>
If hold_duration = None, feedback loop runs indefinitely until KeyBoard Interrupt.

# Control Algorithm
If resistance > fail_resistance (implemented for 24-channel 48V-1A system): <br> 
set voltage to 0V and permanently exclude from control <br>
Elif resistance > target + tolerance: decrease voltage by voltage_step <br>
Elif resistance < target = tolerance: increase voltage by voltage_step <br>
Else: Hold voltage constant <br>
System waits for update_time before next round of feedback control.

# Batch Update Optimisation
For 24-channel 48V-1A system, voltage updates are only sent to Teensy for channels requiring adjustment. <br>
The Teensy then communicates with DACs in batch mode rather than sequentially to minimise communication overhead and ensure synchronised control across all channels.

# State Continuity Between Steps
Voltages: Last voltages from previous step become starting voltages for next step <br>
CSV logging: all steps append to same CSV file <br>
Real-time plots: plotting continues seamlessly across all steps <br>
Failed channels (implemented for 24-channel 48V-1A system): are excluded throughout subsequent steps
