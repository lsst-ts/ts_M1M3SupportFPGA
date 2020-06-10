# HealthAndStatusMemory content

The following is content of _HealthAndStatusMemory_. FPGA target writes 64 U64
into _HealthAndStatusDataFIFO_ when the host writes 2 and 0 into
_HealthAndStatusControlFIFO_.

Data into health and status memory are written by Common Modbus module. Modbus
module source code is the authoritative source for what is written into memory.

| Offset     | Content                                                  |
| ---------- | -------------------------------------------------------- |
| 0-5        | ModBus A data                                            |
| 6-11       | Modbus B data                                            |
| 12-17      | Modbus C data                                            |
| 18-23      | Modbus D data                                            |
| 24-29      | Modbus E data                                            |

## Modbus Data

All data are 32 bit. Offsets are in U64 space.

| Offset | Content                                                      |
| ------ | ------------------------------------------------------------ |
| 0      | Error Flag                                                   |
| 1      | Transmit Bytes Counter                                       |
| 2      | Transmit Frames Counter                                      |
| 3      | Received Bytes Counter                                       |
| 4      | Received Frames Counter                                      |
| 5      | Processed Instructions Counter                               |

### Error Flags

| Bit       | Content                                                   |
| --------  | --------------------------------------------------------- |
| 0(0x01)   | TxInternalFIFO Overflow                                   |
| 1(0x02)   | Invalid instruction                                       |
| 2(0x04)   | Wait for Rx frame timeout                                 |
| 3(0x08)   | Start Bit Error                                           |
| 4(0x10)   | Stop Bit Error                                            |
| 5(0x20)   | RxDataFIFO Overflow                                       |
| 5(0x40)   | Waiting for ModbusTrigger to send data                    |

