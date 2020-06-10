# HealthAndStatusMemory content

The following is content of _HealthAndStatusMemory_. Controller can dump it
into _U16ResponseFIFO_ by writing 254 followed by length into _RequestFIFO_.
The offset is in U16, 2 bytes units.

| Offset     | Content                                                  |
| ---------- | -------------------------------------------------------- |
| 0-19       | ModBus A data                                            |
| 20-39      | Modbus B data                                            |
| 40-59      | Modbus C data                                            |
| 60-79      | Modbus D data                                            |
| 80-99      | Modbus E data                                            |

## Modbus Data

All data are 32 bit. Offsets are in U64 space.

| Offset | Content                                                      |
| ------ | ------------------------------------------------------------ |
| 0      | Error Flag                                                   |
| 1      | Transmit Byte Count                                          |
| 2      | Transmit Frame Count                                         |
| 3      | Received Byte Count                                          |
| 4      | Received Frame Count                                         |

### Error Flags

| Bit       | Content                                                   |
| --------  | --------------------------------------------------------- |
| 0(0x01)   | TxInternalFIFO Overflow                                   |
| 1(0x02)   | Invalid instruction                                       |
| 2(0x04)   | Wait for Rx frame timeout                                 |
| 3(0x08)   | Start Bit Error                                           |
| 4(0x10)   | Stop Bit Error                                            |
| 5(0x20)   | RxDataFIFO Overflow                                       |

