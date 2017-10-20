Source document: https://www.nxp.com/docs/en/user-guide/141520.pdf 

# 6.2 PN532 Host controller communication protocol

Communication between the host controller and the PN532 is performed through frames, in a half-duplex mode.
Four different types of frames are used in one or both directions (host controller to the PN532 and PN532 to the host controller).


## 6.2.1.1 Normal information frame

Information frames are used to convey:
* Commands from the host controller to the PN532,
* And responses to these commands from the PN532 to the host controller.

### The structure of this frame is the following:

![](https://i.imgur.com/P9gbxho.png)


*   **PREAMBLE** — 1 byte (0x00)
*  **START CODE** — 2 bytes (0x00 and 0xFF)
*  **LEN** — 1 byte indicating the number of bytes in the data field (TFI and PD0 to PDn)
*  **LCS** — 1 Packet Length Checksum LCS byte that satisfies the relation: Lower byte of [LEN + LCS] = 0x00
*  **TFI** — 1 byte frame identifier, the value of this byte depends on the way of the message:  
             - **D4h** in case of a frame from the host controller to the PN532  
              - **D5h** in case of a frame from the PN532 to the host controller  
*  **DATA** — LEN-1 bytes of Packet Data Information The first byte PD0 is the Command Code
*  **DCS** — 1 Data Checksum DCS byte that satisfies the relation: Lower byte of [TFI + PD0 + PD1 + ... + PDn + DCS] = 0x00
*  **POSTAMBLE** — 1 byte (0x00)

The amount of data that can be exchanged using this frame structure is limited to 255 bytes (including TFI).

## 6.2.1.2 Extended information frame

The information frame has an extended definition allowing exchanging more data between the host controller and the PN532.
In the firmware implementation of the PN532, the maximum length of the packet data is limited to 264 bytes (265 bytes with TFI included).

### The structure of this frame is the following:
![](https://i.imgur.com/5sw4y8Q.png)
