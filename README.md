# ts_M1M3SupportFPGA

## Description

This repo contains the LabVIEW 2018 FPGA design for the M1M3 Support System FPGA used by the **ts_M1M3Support** software.

## Build Instructions

Building the FPGA takes just about an hour. It is pretty common for the FPGA build process to get stuck on the *Generate Xilinx IP* step. To get around this issue rebuilding an empty VI seems to fix the issue. If the *Generate Xilinx IP* step takes longer than 30 minutes it probably isn't going to complete so abort the compilation and repeat starting at step 6.

1. Open LabVIEW 2018.
2. Open ts_M1M3SupportFPGA.lvproj
3. Expand RT CompactRIO Target
4. Expand FPGA Target
5. Expand Build Specifications
6. Right-Click -> Rebuild on Junk
7. Right-Click -> Rebuild on ts_M1M3SupportFPGA

## Overview

This FPGA design is makes heavy use of FIFOs and **S**ingle **C**ycle **T**imed **L**oop or **SCTL** to move data around the design and keep the size of the design to a minimum. It is highly recommended that before attempting to make a change to the design that you familiarize yourself with the quirks within LabVIEW regarding these two mechanisms.

The overall design starts with many processes that generate samples and then place those samples into a FIFO unique to each process. While that process happens the **Telemetry** process is constantly trying to update the memory segment with the most recent samples for all processes within the design.

**DigitalInput** is very simple process that takes the digital input signals and samples them every 0.200 ms. The trigger for the process can be found under *DigitalInput/Support/DigitalInputTrigger.vi* which produces a trigger every 0.200ms and then waits for the sample process to complete. Once a trigger is produced the *DigitalInput/Support/DigitalInputSampleLoop.vi* will read the current timestamp and state of all digital inputs and place that sample into a FIFO. Then at some other point in time the *Telemetry/Support/TelemetryUpdate.vi* will call the *DigitalInput/TryUpdateDigitalInputSample.vi* to move break up the sample into multiple actions for the telemetry update process to execute and will notify the trigger process that the sample has been fully processed.

Since a SCTL only allows a FIFO to be written to in one loop and read from one loop the design utilizes multiple FIFOs to get around this restriction. In the example above a digital input sample is pushed into the *DigitalInputTelemetryFIFO* so that the it can be read by the *Telemetry/CopyToTelemetryFIFO.vi* loop and pushed into the global *TelemetryFIFO* which doesn't run inside a SCTL.

The force actuator and hardpoint actuator modbus processes are much more complex and rely on the host machine to parse the data.