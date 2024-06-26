# Remote ID Sniffing Guide

This guide provides instructions for setting up and using the Sonoff Sniffle dongle to capture and analyze Remote ID data, specifically focusing on Bluetooth 5 Long Range Extended. The guide assumes you have already flashed your Sonoff Sniffle compatible dongle with the latest Sniffle release.

## Setup for Running Headless in DragonOS

1. Open a terminal and navigate to your home directory:
    ```bash
    cd ~
    ```

2. Clone the necessary repositories:
    ```bash
    git clone https://github.com/nccgroup/Sniffle.git
    git clone -b headless https://github.com/alphafox02/wireshark-dissector.git
    ```

## Bluetooth Sniffing

### Step-by-Step Instructions

1. Open two terminal windows.

2. In the first terminal window, create a FIFO and start the Sniffle receiver:
    ```bash
    mkfifo /tmp/pcap_fifo
    cd ~/Sniffle/python_cli
    ./sniff_receiver.py -l -e -o /tmp/pcap_fifo
    ```
    The above commands have been confirmed to capture Bluetooth 5 extended frames/long range.

3. In the second terminal window, start `tshark` with the Wireshark dissector:
    ```bash
    cd ~/wireshark-dissector
    tshark -r /tmp/pcap_fifo -X lua_script:opendroneid-dissector.lua -T json
    ```
    If you receive Remote ID Bluetooth 5 Long Range/Extended packets, you will see them being decoded in the second window.

4. (Optional) If you want to pipe the output to a remote host, you can add the following at the end of the `tshark` command:
    ```bash
    | nc -u remote_host 12345
    ```

## Additional Capabilities

While this guide focuses on capturing Bluetooth 5 Long Range Extended, it's important to note that the Wireshark dissector supports various protocols, including:
- Wi-Fi Beacon
- Wi-Fi NAN
- Bluetooth 4
- Bluetooth 5

With some different options, Sniffle can potentially capture Bluetooth 4 Remote ID packets as well.

## Additional Information

For more details on how to use the tools and interpret the captured data, refer to the respective documentation of Sniffle and the Wireshark dissector.

---

*Note: Ensure you have all necessary dependencies installed and configured on your DragonOS system before proceeding with the steps above.*
