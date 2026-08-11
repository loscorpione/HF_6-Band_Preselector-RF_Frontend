HF 6-Band Preselector – RF Frontend

This project is a 6-band HF RF preselector designed to improve the selectivity and dynamic performance of HF receivers, particularly homebrew and direct-conversion receivers.

The preselector covers the frequency range from 200 kHz to 30 MHz through six LC band-pass filters. The appropriate filter is selected automatically according to the band selected on the VFO.

The project was developed as an RF frontend for my ESP32 + Si5351 VFO/BFO, creating an integrated and automatically controlled RF signal chain.

In addition to the band-pass filters, the board includes a selectable 0 / -20 dB RF attenuator, controlled directly by the VFO.

## 🎬 Video del progetto

[![HF 6-Band Preselector](https://img.youtube.com/vi/hL2VTXRHFpw/maxresdefault.jpg)](https://youtu.be/hL2VTXRHFpw)

▶️ **Guarda il video completo su YouTube**

Frequency coverage
Filter	Frequency range	Main applications
1	0.2 – 1 MHz	Long Wave, Medium Wave, 630 m
2	1 – 2 MHz	Medium Wave, 160 m
3	2 – 4 MHz	80 m, 60 m
4	4 – 8 MHz	40 m, Short Wave
5	8 – 15 MHz	30 m, 20 m, Short Wave
6	15 – 30 MHz	17 m, 15 m, 12 m, 10 m, Short Wave
Filter switching

The six filters are selected using RF relays controlled by a CD4028 decoder and an ULN2003 Darlington transistor array.

The VFO sends the selected band information to the CD4028, which activates the corresponding output. The ULN2003 then drives the relay coil, allowing the correct RF filter to be connected.

This approach reduces the number of control lines required between the VFO and the RF frontend while keeping the RF switching section simple and reliable.

RF attenuator

The board also includes a selectable 0 / -20 dB attenuator based on a 50 Ω resistive network.

The attenuator can be controlled directly from the VFO and can be useful when the receiver is overloaded by strong signals or out-of-band energy.

Testing

The filters were tested using a NanoVNA, measuring their frequency response, insertion loss and bandwidth.

The measured responses do not perfectly match the calculated values. The differences are most likely related to component tolerances, particularly the inductors.

Further adjustments to the filter component values are therefore planned to improve the correspondence between the calculated and measured responses.

Hardware

The PCB was designed with particular attention to RF layout:

short RF tracks;
extensive ground plane;
physically separated filter sections;
relays positioned close to the RF paths;
50 Ω RF input and output;
dedicated filtering for each frequency range.
Status

Working prototype – further RF optimization planned.

This repository contains the hardware design files, documentation and test information for the project.

Project by ScorpioneMaker
Electronics • RF • Amateur Radio • DIY
