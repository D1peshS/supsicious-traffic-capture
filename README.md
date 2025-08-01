#!/bin/bash
echo "[*] Monitoring and logging suspicious ports..."
echo "[*] Logs written to suspicious.log"

sudo tcpdump -i wlan0 port 21 or port 22 or port 23 or port 80 or port 443 -l -nn |
while read line; do
  echo "$(date): $line" >> suspicious.log
  
  # OPTIONAL — send SMS if something serious hits port 23 (Telnet)
  echo "$line" | grep "23" && python3 send_sms.py &
done
