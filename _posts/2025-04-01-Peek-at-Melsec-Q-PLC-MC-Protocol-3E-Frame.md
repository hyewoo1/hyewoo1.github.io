---
title: "Peek at Melsec Q PLC MC Protocol 3E Frame"
categories:
  - Plc
tags:
  - Plc
  - MC Protocol
---

## Peek at Melsec Q PLC MC Protocol 3E Frame


### Reference
- Q Corresponding MELSEC Communication Protocol Reference Manual
- QnUCPU User's Manual (Communication via Built-in Ethernet Port)



### Use Tools
- y-a-terminal(https://sourceforge.net/projects/y-a-terminal/)

---

### Command (Sub Command)

| Function    | Bit/Word    | Command (Sub Command) | Processing Description    |
|-------------|-------------|-----------------------|---------------------------|
| **Bulk Read**     | Bit Unit       | `0401(0001)`          | Read bit devices one point at a time                             |
|                   | Word Unit      | `0401(0000)`          | Read bit devices in bulk (16 points at a time)                  |
|                   |                |                       | Read word devices one point at a time                            |
| **Bulk Write**    | Bit Unit       | `1401(0001)`          | Write to bit devices one point at a time                         |
|                   | Word Unit      | `1401(0000)`          | Write to bit devices in bulk (16 points at a time)              |
|                   |                |                       | Write to word devices one point at a time                        |
| **Random Read**    | Word Unit      | `0403(0000)`          | Randomly read bit devices, specifying 16 or 32 points           |
|                   |                |                       | Randomly read word devices, specifying 1 or 2 points             |
| **Random Write**    | Bit Unit       | `1402(0001)`          | Write to bit devices one point at a time                         |
|                   | Word Unit      | `1402(0000)`          | Write to bit devices in bulk (16 points at a time)              |
|                   |                |                       | Write to word devices, specifying 1 or 2 points                  |
| **Monitor Data Registration** | Data Registration | `0801(0000)`          | Register monitor bit devices in bulk (16 points at a time)      |
|                   | Word Unit      |                       | Register monitor word devices, specifying 1 or 2 points          |
| **Monitor**       | Word Unit      | `0802(0000)`          | Monitor the registered devices                                     |
| **Multiple Blocks**| Bulk Read      | `0406(0000)`          | Randomly read multiple blocks, specifying n words of either word or bit devices |
| **Multiple Blocks**| Bulk Write     | `1406(0000)`          | Randomly write multiple blocks, specifying n words of either word or bit devices |

---

### Devices

| Device Name            | Symbol | Type   | Notation | ASCII | Binary |
|------------------------|--------|--------|----------|-------|--------|
| Special Relay          | SM     | Bit    | Decimal  | SM    | 91H    |
| Special Register        | SD     | Word   | Decimal  | SD    | A9H    |
| Input                  | X      | Bit    | Hexadecimal | X*  | 9CH    |
| Output                 | Y      | Bit    | Hexadecimal | Y*  | 9DH    |
| Internal Relay         | M      | Bit    | Decimal  | M*    | 90H    |
| Latch Relay            | L      | Bit    | Decimal  | L*    | 92H    |
| Simulator              | F      | Bit    | Decimal  | F*    | 93H    |
| Edge Relay             | V      | Bit    | Decimal  | V*    | 94H    |
| Link Relay             | B      | Bit    | Hexadecimal | B*  | A0H    |
| Data Register          | D      | Word   | Decimal  | D*    | A8H    |
| Link Register          | W      | Word   | Hexadecimal | W*  | B4H    |
| Timer (Increasing)     | TS     | Bit    | Decimal  | TS    | C1H    |
| Timer (Coil)           | TC     | Bit    | Decimal  | TC    | C0H    |
| Timer (Current Value)  | TN     | Word   | Decimal  | TN    | C2H    |
| Cumulative Timer (Increasing) | STS | Bit | Decimal | SS    | C7H    |
| Cumulative Timer (Coil)| STC    | Bit    | Decimal  | SC    | C6H    |
| Cumulative Timer (Current Value) | STN | Word | Decimal | SN    | C8H    |
| Counter (Increasing)   | CS     | Bit    | Decimal  | CS    | C4H    |
| Counter (Coil)        | CC     | Bit    | Decimal  | CC    | C3H    |
| Counter (Current Value) | CN    | Word   | Decimal  | CN    | C5H    |
| Link Special Relay     | SB     | Bit    | Hexadecimal | SB  | A1H    |
| Link Special Register   | SW     | Word   | Hexadecimal | SW  | B5H    |
| Direct Access Input     | DX     | Bit    | Hexadecimal | DX  | A2H    |
| Direct Access Output    | DY     | Bit    | Hexadecimal | DY  | A3H    |
| Index Register          | Z      | Word   | Decimal  | Z*    | CCH    |
| File Register           | R      | Word   | Decimal  | R*    | AFH    |
| Block Switching Method   | ZR     | Word   | Hexadecimal | ZR  | B0H    |
| Extended Data Register   | D      | Word   | Decimal  | D*    | A8H    |
| Extended Link Register    | W      | Word   | Hexadecimal | W*  | B4H    |

---

### Error Codes

| No. | Error Code (Hex) | Error Description                                                                 | Handling Method                                                                               |
|-----|------------------|-----------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------|
| 1   | 4000H ~ 4FFFH    | Error from the CPU module during communication (error occurred outside MC protocol functionality) | Refer to the QCPU User Manual (Hardware Design and Maintenance Inspection) to handle it.  |
| 2   | 0055H            | An attempt was made to write data to the CPU module during RUN, which is not allowed. | Set permission for writing during RUN, then write the data. Alternatively, set the CPU module to STOP before writing data. |
| 3   | C050H            | Received data that could not be converted to binary code when setting ASCII codes in the protocol port. | Modify the communication data code settings to binary, then resend to the Ethernet port built-in QCPU. |
| 4   | C051H ~ C054H    | Read/write integer exceeds allowable range.                                      | Correct the read/write integer and resend to the Ethernet port built-in QCPU.               |
| 5   | C056H            | A read/write request exceeded the maximum address.                               | Adjust the starting address or read/write integer, and resend to the Ethernet port built-in QCPU. |
| 6   | C058H            | The requested data length after ASCII to binary conversion does not match the character (part of text). | Review the text content or required data length and correct it, then resend to the Ethernet port built-in QCPU. |
| 7   | C059H            | - Command or sub-command has been incorrectly specified. <br> - Command or sub-command not available on Ethernet port built-in QCPU. | Review the requested content and specify commands and sub-commands that are applicable for the Ethernet port built-in QCPU. |
| 8   | C05BH            | Cannot read or write to the specified device on the Ethernet port built-in QCPU. | Review the devices being read or written.                                                   |
| 9   | C05CH            | There is an error in the requested content. (Reading/writing bits on a word device) | Modify the request and resend to the Ethernet port built-in QCPU. (Modify the sub-command) |
| 10  | C05DH            | Monitoring is not registered.                                                    | Register the monitor and then activate it.                                                  |
| 11  | C05FH            | The request cannot be executed for the target CPU module.                        | Modify the network number, PLC number, requested corresponding module I/O number, and requested corresponding module country number. Modify the read/write request. |
| 12  | C060H            | There is an error in the requested content. (Error in specifying data for bit devices, etc.) | Modify the request and resend to the Ethernet port built-in QCPU. (Correct the data)       |
| 13  | C061H            | The required data length does not match the number of characters (part of text). | Review the text content or required data length and correct it, then resend to the Ethernet port built-in QCPU. |
| 14  | C06FH            | Received an ASCII station number when the communication data code setting is binary, or received a binary station number when the communication data code setting is ASCII. | Modify the requested station number in the communication data code setting. Change the requested station number to match the communication data code setting. |
| 15  | C070H            | Cannot make memory expansions for large volumes on the device.                   | Perform read/write without making the expansion.                                            |
| 16  | C08BH            | Data specified cannot be handled by the CPU module.                              | Review the requested content and resend the failure request.                                 |
| 17  | C200H            | There is an error in the remote password.                                        | Set the remote password.                                                                      |
| 18  | C201H            | If the remote password used for communication is only set in ASCII code, and it is in factory state, then the reason for the sub-command cannot be converted to binary code. | Release the remote password and reset it.                                                    |
| 19  | C204H            | The device requesting the release of the remote password is different.           | Request the factory setting for the remote password from the device requesting its release.  |

---

### ASCII Request / Response Messages

| Subheader | Network Number | PLC Number | Requested Corresponding Module IO | Requested Corresponding Module Country Number | Requested Data Length | CPU Monitoring Timer (Unit: 250ms) | Command | Sub Command | Device Code | Leading Device | Device Points |
|-----------|----------|------------|-------------------|-------------|----------------------|--------------|---------|-------------|--------------|------|------|
| 5000      | 00             | FF         | 03FF      | 00             | 0018      | 0010     | 0401    | 0001        | M*           | 001000         | 0005           |

- Read 5 bits from M1000.
- The requested data length refers to the length from `CPU Monitoring Timer` to `Data`.

| Subheader | Network Number | PLC Number | Requested Corresponding Module IO | Requested Corresponding Module Country Number | Requested Data Length | Response Code | Data  |
|-----------|----------------|------------|--------------------|--------------|----------------------|----------------|-------|
| D000      | 00             | FF         | 03FF                              | 00           | 0009                 | 0000           | 11001 |

- Response to reading 5 bits from M1000.
- M1000: `1`, M1001: `1`, M1002: `0`, M1003: `0`, M1004: `1`.
- The bit information is separated into `0` and `1` from left to right in the response.


| Subheader | Network Number | PLC Number | Requested Corresponding Module IO | Requested Corresponding Module Country Number | Requested Data Length | Error Code | Response Network Number | Response PLC Number | Requested Corresponding Module IO Number | Requested Corresponding Module Country Number | Command | Sub Command |
|-----------|----------------|------------|-----------------------------------|----------------------------------------------|----------------------|------------|-----------------------|-------------------|------------------------------------------|----------------------------------------------|---------|-------------|
| D000      | 00             | FF         | 03FF                              | 00                                           | 0016                 | C051       | 00                    | FF                | 03FF                                     | 00                                           | 0401    | 0001       |

- An error response was received when the request was made.
- Error Code: `C051`

| Subheader | Network Number | PLC Number | Requested Corresponding Module IO | Requested Corresponding Module Country Number | Requested Data Length | CPU Monitoring Timer (Unit: 250ms) | Command | Sub Command | Device Code | Leading Device | Device Points | Data   |
|-----------|----------------|------------|-----------------------------------|----------------------------------------------|----------------------|------------------------------------|---------|-------------|--------------|----------------|----------------|--------|
| 5000      | 00             | FF         | 03FF                              | 00                                           | 001D                 | 0010                               | 1401    | 0001        | M*           | 001000         | 0005           | 10110  |

- Write 5 bits starting from M1000: `10110`

| Subheader | Network Number | PLC Number | Requested Corresponding Module IO | Requested Corresponding Module Country Number | Requested Data Length | Response Code |
|-----------|----------------|------------|-----------------------------------|----------------------------------------------|----------------------|----------------|
| D000      | 00             | FF         | 03FF                              | 00                                           | 0004                 | 0000           |

- Normal response to data writing.

| Subheader | Network Number | PLC Number | Requested Corresponding Module IO | Requested Corresponding Module Country Number | Requested Data Length | CPU Monitoring Timer (Unit: 250ms) | Command | Sub Command | Device Code | Leading Device | Device Points |
|-----------|----------------|------------|-----------------------------------|----------------------------------------------|----------------------|------------------------------------|---------|-------------|--------------|----------------|----------------|
| 5000      | 00             | FF         | 03FF                              | 00                                           | 0018                 | 0010                               | 0401    | 0000        | M*           | 001000         | 0002           |

- Read 2 words of bit device M1000 in word format.

| Subheader | Network Number | PLC Number | Requested Corresponding Module IO | Requested Corresponding Module Country Number | Requested Data Length | Response Code | Data         |
|-----------|----------------|------------|-----------------------------------|----------------------------------------------|----------------------|----------------|--------------|
| D000      | 00             | FF         | 03FF                              | 00                                           | 000C                 | 0000           | 000D 0000    |

- Response to reading 2 words from the bit device M1000 in word format.
- The leading `000D` corresponds to `M1015` ~ `M1000`, and the trailing `0000` corresponds to `M1031` ~ `M1016`.
- 0x0D = 13 = 1 1 0 1.
- M1000, M1001, and M1003 are ON.

| Subheader | Network Number | PLC Number | Requested Corresponding Module IO | Requested Corresponding Module Country Number | Requested Data Length | CPU Monitoring Timer (Unit: 250ms) | Command | Sub Command | Device Code | Leading Device | Device Points |
|-----------|----------------|------------|-----------------------------------|----------------------------------------------|----------------------|------------------------------------|---------|-------------|--------------|----------------|----------------|
| 5000      | 00             | FF         | 03FF                              | 00                                           | 0018                 | 0010                               | 0401    | 0000        | D*           | 001000         | 0003           |

- Read 3 words starting from D1000.

| Subheader | Network Number | PLC Number | Requested Corresponding Module IO | Requested Corresponding Module Country Number | Requested Data Length | Response Code | Data         |
|-----------|----------------|------------|-----------------------------------|----------------------------------------------|----------------------|----------------|--------------|
| D000      | 00             | FF         | 03FF                              | 00                                           | 0010                 | 0000           | 04D2 162E 3039 |

- Response to reading 3 words from D1000.
- D1000: `1234 (0x04D2)`, D1001: `5678 (0x162E)`, D1002: `12345 (0x3039)`.

| Subheader | Network Number | PLC Number | Requested Corresponding Module IO | Requested Corresponding Module Country Number | Requested Data Length | CPU Monitoring Timer (Unit: 250ms) | Command | Sub Command | Device Code | Leading Device | Device Points | Data        |
|-----------|----------------|------------|-----------------------------------|----------------------------------------------|----------------------|------------------------------------|---------|-------------|--------------|----------------|----------------|-------------|
| 5000      | 00             | FF         | 03FF                              | 00                                           | 0020                 | 0010                               | 1401    | 0000        | M*           | 001000         | 0002           | FF00 00FF   |

- Write 2 words starting from M1000.
- M1015 ~ M1000: `FF00`, M1015 ~ M1012: `0x0F`, M1011 ~ M1008: `0x0F`.
- M1031 ~ M1016: `00FF`.

| Subheader | Network Number | PLC Number | Requested Corresponding Module IO | Requested Corresponding Module Country Number | Requested Data Length | CPU Monitoring Timer (Unit: 250ms) | Command | Sub Command | Device Code | Leading Device | Device Points | Data        |
|-----------|----------------|------------|-----------------------------------|----------------------------------------------|----------------------|------------------------------------|---------|-------------|--------------|----------------|----------------|-------------|
| 5000      | 00             | FF         | 03FF                              | 00                                           | 0020                 | 0010                               | 1401    | 0000        | D*           | 001000         | 0002           | 10E1 265D   |

- Write 2 words starting from D1000.
- D1000: `10E1`, D1001: `265D`.

---
### Binary Request / Response Messages

| Subheader | Network Number | PLC Number | Requested Corresponding Module IO | Requested Corresponding Module Country Number | Requested Data Length | CPU Monitoring Timer (Unit: 250ms) | Command | Sub Command | Leading Device | Device Code | Device Points |
|-----------|----------------|------------|-----------------------------------|----------------------------------------------|----------------------|------------------------------------|---------|-------------|----------------|--------------|----------------|
| 0x5000    | 0x00          | 0xFF       | 0xFF03                            | 0x00                                         | 0x1800               | 0x1000                             | 0x0104  | 0x0100      | 0xE80300       | 0x90        | 0x0500         |

- Read 5 bits starting from M1000.
- The binary format is fundamentally in little-endian.

| Subheader | Network Number | PLC Number | Requested Corresponding Module IO | Requested Corresponding Module Country Number | Requested Data Length | Response Code | Data       |
|-----------|----------------|------------|-----------------------------------|----------------------------------------------|----------------------|----------------|------------|
| 0xD000    | 0x00          | 0xFF       | 0xFF03                            | 0x00                                         | 0x0500               | 0x0000         | 0x110010   |

- Response to reading 5 bits starting from M1000.
- M1000: `1`, M1001: `1`, M1002: `0`, M1003: `0`, M1004: `1`.
- The bit information is responded with `0`s and `1`s in order from left.
- To maintain byte alignment, even if an odd number of bits is read, it is padded with 0. `11001` becomes `110010`.

| Subheader | Network Number | PLC Number | Requested Corresponding Module IO | Requested Corresponding Module Country Number | Requested Data Length | Error Code | Response Network Number | Response PLC Number | Requested Corresponding Module IO Number | Requested Corresponding Module Country Number | Command | Sub Command |
|-----------|----------------|------------|-----------------------------------|----------------------------------------------|----------------------|------------|-----------------------|-------------------|-------------------------------------------|----------------------------------------------|---------|-------------|
| 0xD000    | 0x00          | 0xFF       | 0xFF03                            | 0x00                                         | 0x0B00               | 0x51C0     | 0x00                  | 0xFF                | 0xFF03                                   | 0x00                                         | 0x0114  | 0x0100     |

- An error response was received when the request was made.
- Error Code: `C051`.

| Subheader | Network Number | PLC Number | Requested Corresponding Module IO | Requested Corresponding Module Country Number | Requested Data Length | CPU Monitoring Timer (Unit: 250ms) | Command | Sub Command | Device Code | Leading Device | Device Points | Data       |
|-----------|----------------|------------|-----------------------------------|----------------------------------------------|----------------------|------------------------------------|---------|-------------|--------------|----------------|----------------|------------|
| 0x5000    | 0x00          | 0xFF       | 0xFF03                            | 0x00                                         | 0x0F00               | 0x1000                             | 0x0114  | 0x0100      | 0xE80300     | 0x90          | 0x0500       | 0x101100   |

- Write 5 bits starting from M1000: `10110`.

| Subheader | Network Number | PLC Number | Requested Corresponding Module IO | Requested Corresponding Module Country Number | Requested Data Length | Response Code |
|-----------|----------------|------------|-----------------------------------|----------------------------------------------|----------------------|----------------|
| 0xD000    | 0x00          | 0xFF       | 0xFF03                            | 0x00                                         | 0x0200               | 0x0000         |

- Normal response to data writing.

| Subheader | Network Number | PLC Number | Requested Corresponding Module IO | Requested Corresponding Module Country Number | Requested Data Length | CPU Monitoring Timer (Unit: 250ms) | Command | Sub Command | Leading Device | Device Code | Device Points |
|-----------|----------------|------------|-----------------------------------|----------------------------------------------|----------------------|------------------------------------|---------|-------------|----------------|--------------|----------------|
| 0x5000    | 0x00          | 0xFF       | 0xFF03                            | 0x00                                         | 0x0C00               | 0x1000                             | 0x0104  | 0x0000      | 0xE80300       | 0x90        | 0x0200         |

- Read 2 words of the bit device M1000 in word format.

| Subheader | Network Number | PLC Number | Requested Corresponding Module IO | Requested Corresponding Module Country Number | Requested Data Length | Response Code | Data               |
|-----------|----------------|------------|-----------------------------------|----------------------------------------------|----------------------|----------------|--------------------|
| 0xD000    | 0x00          | 0xFF       | 0xFF03                            | 0x00                                         | 0x0600               | 0x0000         | 0x19 0x08 0x22 0x62 |

- Response to reading 2 words of the bit device M1000.
- M1000 ~ M1003: `9`, M1004 ~ M1007: `1`, M1008 ~ M1011: `8`, M1012 ~ M1015: `0`.
- Data is organized in the order of `1HL`, `2HL`, `3HL`, `4HL`.

| Subheader | Network Number | PLC Number | Requested Corresponding Module IO | Requested Corresponding Module Country Number | Requested Data Length | CPU Monitoring Timer (Unit: 250ms) | Command | Sub Command | Leading Device | Device Code | Device Points |
|-----------|----------------|------------|-----------------------------------|----------------------------------------------|----------------------|------------------------------------|---------|-------------|----------------|--------------|----------------|
| 0x5000    | 0x00          | 0xFF       | 0xFF03                            | 0x00                                         | 0x0C00               | 0x1000                             | 0x0104  | 0x0000      | 0xE80300       | 0xA8        | 0x0300         |

- Read 3 words starting from D1000.

| Subheader | Network Number | PLC Number | Requested Corresponding Module IO | Requested Corresponding Module Country Number | Requested Data Length | Response Code | Data               |
|-----------|----------------|------------|-----------------------------------|----------------------------------------------|----------------------|----------------|--------------------|
| 0xD000    | 0x00          | 0xFF       | 0xFF03                            | 0x00                                         | 0x0800               | 0x0000         | 0xD204 0x2E16 0x3423 |

- Response to reading 3 words starting from D1000.
- D1000: `1234 (0x04D2)`, D1001: `5678 (0x162E)`, D1002: `9012 (0x2334)`.

| Subheader | Network Number | PLC Number | Requested Corresponding Module IO | Requested Corresponding Module Country Number | Requested Data Length | CPU Monitoring Timer (Unit: 250ms) | Command | Sub Command | Device Code | Leading Device | Device Points | Data               |
|-----------|----------------|------------|-----------------------------------|----------------------------------------------|----------------------|------------------------------------|---------|-------------|--------------|----------------|----------------|--------------------|
| 0x5000    | 0x00          | 0xFF       | 0xFF03                            | 0x00                                         | 0x1000               | 0x1000                             | 0x0114  | 0x0000      | 0xE80300     | 0x90          | 0x0200         | 0xFF 0x0F 0xF0 0x0F |

- Write 2 words starting from M1000.
- Data is organized in the order of `1HL`, `2HL`, `3HL`, `4HL`.

| Subheader | Network Number | PLC Number | Requested Corresponding Module IO | Requested Corresponding Module Country Number | Requested Data Length | CPU Monitoring Timer (Unit: 250ms) | Command | Sub Command | Device Code | Leading Device | Device Points | Data               |
|-----------|----------------|------------|-----------------------------------|----------------------------------------------|----------------------|------------------------------------|---------|-------------|--------------|----------------|----------------|--------------------|
| 0x5000    | 0x00          | 0xFF       | 0xFF03                            | 0x00                                         | 0x1000               | 0x1000                             | 0x0114  | 0x0000      | 0xE80300     | 0xA8          | 0x0200         | 0x4D05 0x8530       |

- Write 2 words starting from D1000.
- D1000: `1357 (0x054D)`, D1001: `12421 (0x3085)`.

---

### Test Binary with y-a-terminal
```
(21:15:25.892) 50h 00h 00h FFh FFh 03h 00h 0Fh 00h 10h 00h 01h 14h 01h 00h E8h 03h 00h 90h 05h 00h 10h 01h 10h
(21:15:25.946) D0h 00h 00h FFh FFh 03h 00h 02h 00h 00h 00h
(21:15:29.199) 50h 00h 00h FFh FFh 03h 00h 0Ch 00h 10h 00h 01h 04h 01h 00h E8h 03h 00h 90h 05h 00h
(21:15:29.240) D0h 00h 00h FFh FFh 03h 00h 05h 00h 00h 00h 10h 01h 10h
(21:15:32.819) 50h 00h 00h FFh FFh 03h 00h 10h 00h 10h 00h 01h 14h 00h 00h E8h 03h 00h A8h 02h 00h 4Dh 05h 85h 30h
(21:15:32.873) D0h 00h 00h FFh FFh 03h 00h 02h 00h 00h 00h
(21:15:34.460) 50h 00h 00h FFh FFh 03h 00h 0Ch 00h 10h 00h 01h 04h 00h 00h E8h 03h 00h A8h 03h 00h
(21:15:34.497) D0h 00h 00h FFh FFh 03h 00h 08h 00h 00h 00h 4Dh 05h 85h 30h 00h 00h
(21:15:36.993) 50h 00h 00h FFh FFh 03h 00h 10h 00h 10h 00h 01h 14h 00h 00h E8h 03h 00h 90h 02h 00h FFh 0Fh F0h 0Fh
(21:15:37.037) D0h 00h 00h FFh FFh 03h 00h 02h 00h 00h 00h
(21:15:38.729) 50h 00h 00h FFh FFh 03h 00h 0Ch 00h 10h 00h 01h 04h 00h 00h E8h 03h 00h 90h 02h 00h
(21:15:38.781) D0h 00h 00h FFh FFh 03h 00h 06h 00h 00h 00h FFh 0Fh F0h 0Fh
```

---
### Test Ascii with y-a-terminal
```
(21:19:00.848) 500000FF03FF00001D001014010001M*001000000510110
(21:19:01.070) D00000FF03FF0000040000
(21:19:02.131) 500000FF03FF000018001004010001M*0010000005
(21:19:02.173) D00000FF03FF000009000010110
(21:19:03.262) 500000FF03FF000020001014010000D*001000000210E1265D
(21:19:03.307) D00000FF03FF0000040000
(21:19:04.196) 500000FF03FF000018001004010000D*0010000003
(21:19:04.235) D00000FF03FF000010000010E1265D0000
(21:19:04.740) 500000FF03FF000020001014010000M*0010000002FFFFFFFF
(21:19:04.784) D00000FF03FF0000040000
(21:19:05.344) 500000FF03FF000018001004010000M*0010000002
(21:19:05.394) D00000FF03FF00000C0000FFFFFFFF
```
---
