Usually used on unlicensed channels. It a Long range Low power technology(maximum achieved 1336 km). Based on chirp spread
spectrum (CSS)

## LoRa Modulation (Chirp)
LoRa is a Spread Spectrum modulation scheme (Chirp Spread Spectrum), where the
symbole enrgy is spread across a wide Bandwidth

![](https://i.imgur.com/WZoVarm.png)

## What is a symbol ?
A symbol is the smallest unit of data that can be transmitted over a communication system. It represents a specific state in a modulation scheme, such as amplitude, frequency, or phase.

![](https://i.imgur.com/oDteZuT.png)

This is useful as we can encode the data in the frequency itself and not only have to decode the signal but only recognising the frequency.
To create new symbols LoRa can just shift the signal(the number of possible symbols depends on the accuracy and possibility of your device).
## LoRa (Spreading Factor)
Number of symbols I can include in my transmission, the more symbols there are the slower will be the transmission.
We transmit a sample every: $T=\frac{1}{BW}$ A symbol s(nTs) is sent every $T_s=2^{SF}T$

![](https://i.imgur.com/ioTibHz.png)

The spreading factor is changing the angular coefficient and spreading the possible recognizing of the symbol. 
## LoRa (Frequency Shift Chirp Modulation)
They have very interesting properties, but they are chirp with shifting.
$$N_{symbols}=2^{SF}=\{0,1,...,2^{SF}-1\},SF=SpreadingFactor(7-12)$$
![](https://i.imgur.com/w4IWmHE.png)

A possible way to encode using these chirps is through encoding symbols.
## LoRa Demodulation
To demodulate the LoRa symbosl, compare with a pre-defined set of symbols and the one with higher similarity score wins. (But is inefficient)
We also need a way to synchronize and notify the receiver that is going to receive a LoRa frame

![](https://i.imgur.com/ZpFEfKq.png)

There is also the need to synchronize the code.
## LoRa Demodulation - Synchronization
A preamble is sent which consist in 6 repeated base symbols.
Since the base symbo is sent, even if the receiver is not in sync, he will read it like
a data symbol repeated 6 time. This is the indication of the preamble. This data symbol read is proportional to the misalignment. Just need to understand the symbols. This is done through a property:
![](https://i.imgur.com/V5haMb8.png)

By changing the spreading factor I can create orthogonality: enabling transmission on the same channel at the same time without collision. 
## LoRa Modulation
Signal is chipped at a higher data rate and modulated onto a chirp signal
Key Properties
- Long range
- High robustness
- Multipath resistance
- Doppler resistance
- Low power
 LoRa coding rate (CR) is the ratio of information bits over transmitted bits. 4/5 means for each 4 bit of data I need to transmit 5 bits. 
 $$\text{Chip rate: } R_c=BW[chips/s]$$
 $$\text{Bit rate: }R_b=SF\frac{\frac{4}{4+CR}}{\frac{2^{SF}}{BW}}[b/s] $$
## LoRa Parameters
 1. Transmission Power (TP): −4 dBm to 20 dBm (1 dBm steps)
	Subject to hardware limitation
2. Carrier Frequency (CF): 137 MHz to 1020 MHz (61 MHz steps)
3. Spreading Factor (SF): the ratio between the symbol rate and chip rate (7 to 12)
	Higher spreading factor -> Higher SNR -> Higher range and reliability -> Higher airtime of the packet -> Lower transmission rate
4. Bandwidth (BW): the width of frequencies in the transmission band
	Higher BW -> Higher rate -> Lower reliability (additional noise)
5. Coding Rate (CR):
	 Forward Error Correction (FEC) rate values in [4/5, 4/6, 4/7, 4/8]
## Successful transmission
1. Communication Range
	Packet is received if the RX **Power** is higher than **Sensitivity**
	 Many factors influencing: $P_{TX}$, Path Loss, TX and RX Gains, BW, SF
		PRX > SRX
2. Collisions: Even if received, the transmission is not successful if there are collisions
	Reception Overlap
		![](https://i.imgur.com/nVSeN4R.png)

		 Overlap if |mx − my| < dx + dy

	Carrier Frequency:
		different CF -> No interference    Overlap if |fx − fy| < fthresh
	Spreading Factor:
		different SF -> No interference    Collision if SFx = SFy
	