# opendroneid-wireshark-dissector

[Wireshark](https://www.wireshark.org) dissector plugin to parse and analyze captured Open Drone ID packets

It currently supports Wi-Fi Beacon, Wi-Fi NAN and Bluetooth 4 and Bluetooth 5. This guide assumes you have already flashed your Sonoff Sniffle compatible donge with the latest Sniffle release

### Guide

1. Setup for running headless in DragonOS
    1. Cd ~
    2. git clone https://github.com/nccgroup/Sniffle.git
    3. git clone -b headless https://github.com/alphafox02/wireshark-dissector.git
    

2. Bluetooth Sniffing
    1.  Open two terminal windows and run the following in the first window
    2.  mkfifo /tmp/pcap_fifo
    3.  cd ~/Sniffle/python_cli
    4.  ./sniff_receiver.py -l -e -o /tmp/pcap_fifo
    5.  The above commands have been confirmed to capture BT5 extened frames/long range.
    6.  In the second window run the following
    7.  cd ~/wireshark-dissector
    8.  tshark -r /tmp/pcap_fifo -X lua_script:opendroneid-dissector.lua -T json
    9.  If you receive RID BT5 LR/Ext packets you will see it being decoded in the second window
    10. If you want to pipe out, you can add the following at the end of step 8
    11. | nc -u remote_host 12345
