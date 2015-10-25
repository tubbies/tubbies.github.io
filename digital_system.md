# Digital System

## Overview
***
- To review basics of digital design

## Logic types
***

#### ** Combinational Logic **
 
- Digital logic that output is only function of current input

#### ** Sequential Logic **

- Digital logic that output is function both current input or previous input(or states)
- States are stored in memory components such as latch or flip-flop in synchronous digital design
- **Latch** : Level-sensitive (Change data when level is high or low)
- **Flip-Flop**: Edge-sensitive (Change data when positive / negative edge)

## ** Synchronous vs Asynchronous design **
***

#### ** Synchronous circuit **
- Digital circuit is synchronized by clock signal

#### ** Asynchronous circuit ** 
- Digital circuit is not governed by clock signal and transferred by specific transfer protocol (such as handshaking)

## ** Clock **
***

- Synchronization signal for synchronous digital circuit. Usually have 50% duty cycle

- ** Skew ** : Spatial deviation of clock signal in two different logic components

- ** Jitter ** : Temporal deviation of clock signal in one logic component

- ** Setup time ** : The minimum amount of time the data should be held steady before clock event  (Recovery time is setup time of asynchronous port)

- ** Hold time ** : The minimum amount of time the data should be held steady after clock event (Removal time is hold time of asynchronous port)

## ** Sign-off Check **
***

- ** DRC ** (Design Rule Check) / ** LVS ** (Layout Versus Schematic)

- ** Formal verification ** : Provides correctness of algorithm by solving math modeling of Each step (Such as RTL, Synthesized netlist, Post-layout netlist)

- ** Static timing analysis ** : Timing analysis (w/o simulation) of various phase of design (synthesis, CTS, DFT, P&R, in-place routing)

- ** Voltage drop analysis ** : Analyze IR-Drop and effect of decoupling capacitance (capacitance b/w power source and power distribution network)

- ** Signal integrity  ** : Analyze noise (such as ringing, crosstalk, power supply noise)