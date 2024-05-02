# RX configuration
[<--- BACK TO MAIN PAGE](../README.md)
1. [How the uA currents are achieved in RX](#init)
2. [RX sampling configuration window](#rx_conf)
3. [Why TX preamble size is important](#preamble_desc)
4. [How to configure preamle size on your lora tracker](#tracker_cfg)
5. [Multiple Frequency Listening](#multi_rx)
6. [Power consumption measurements](#power_measurements)

<div id="init"></div>  

## How the uA currents are achieved in RX 

The uDigi receiver operates in a sampled RX mode - it wakes up for a very short period to check if signal is present. This mode allows to lower an average current lower than the radio chip's declared continuous RX current.

![Current vs time](./resources/img/current_vs_time.png)  
![Average rx current](./resources/img/pp_350_rx.png)

Chart above shows how current changes vs time. Here are the main principles for power consumption:
* The average current depends strictly on the time configured between RX samples.
* Optimal power consumption is achieved when the sampling period is set as constant (digi also supports an adaptive period).

<div id="rx_conf"></div>  

## RX sampling configuration window
![sampling rx config window](./resources/img/settings_advanced_radio.png)  

### Constant sampling configuration
To configure constant sampling period, set both **RX interval MAX** and  **RX interval MIN** to the same value. For example to set sampling period to 350 ms:
* **RX interval MAX**  = 350
* **RX interval MIN**  = 350

### Adaptative sampling
When other lora devices like trackers transmit short preamble packets, to achieve better receive effectiveness, you can configure the adaptive mode. TThis mode reduces the sampling interval when APRS traffic is detected, increasing the possibility of receiving signals but also increasing the current usage. It can reduce the period to as short as the configured **RX Interval MIN**. In other hand, when aprs traffic is small, software autmaticly extends sampling interval to **RX interval MAX** which lowers power consumption.

![Adaptative sampling current chart](./resources/img/adaptative_sampling.png)

<div id="preamble_desc"></div>  

## Why TX preamble size is important
The TX preamble length on devices that want to use your digipeater has a significant impact on the receive effectiveness of the digi. 

![Lora packet structure](./resources/img/lora_packet_structure.png)

Each packet is preceded by a preamble that allows the receiver to synchronize and provides information that a header and payload will soon be transmitted. When the digi detects a preamble, it turns on the receiver for a short time to receive the full packet.

### Bad and good configurations
![preamble effectiveness](./resources/img/preamble_effectiveness.png)  

<div id="tracker_cfg"></div>  

## How to configure preamle size on your lora tracker
If your digi is configured to work with long sample interval (good for low power consumption), your tracker device should transmit long enought preamble, to have 100% chance of beeing digipeated. (Explained on the image above).  

[TTGO-T-Beam-LoRa-APRS](https://github.com/dl9sau/TTGO-T-Beam-LoRa-APRS) this software for TTGO trackers and digipeaters allows you to set custom preamble size. 
![ttgo preamle len](./resources/img/ttgo_preamble_len_settings.png)

Symbol time for LORA 1200 is 4.1 ms, and for LORA 300 is 32.77 ms.  
Ex.: if you configured uDigi with fixed rx sampling interval of 350 ms, you should set TX preamble len on your tracker to 11 symbols for LORA 300, or 86 symbols for LORA 1200.

<div id="multi_rx"></div>  

## Multiple Frequency Listening
While using sampling rx mode, it is possible to listen simultaneously on different frequencies and lora modes.

![Rx prescalers settings](./resources/img/settings_rx_psc.png)
You can adjust the ratio of listened frequencies using the **Rx PSC** settings.

![Multi freq rx](./resources/img/current_vs_time_multi_rx.png)

<div id="power_measurements"></div>  

## Power consumption measurements
Measurements was taken with digi settings:
* **RX interval MAX**  = 350
* **RX interval MIN**  = 350

Tested under 3.5 V input voltage.

### Sleep current
![Sleep current](./resources/img/pp_sleep.png)

### Average digi current in rx
![Average rx current](./resources/img/pp_350_rx.png)

