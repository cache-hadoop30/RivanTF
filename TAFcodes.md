# autowifi-json.py

import json
from netmiko import ConnectHandler

with open('autoAP-jsn.json') as f:
    data = json.load(f)

conn = ConnectHandler(**data['aironetInfo'])
conn.enable()

output = conn.send_config_set([
    f"hostname {data['aironetConfig']['hostname']}",
    "default interface Dot11Radio0",
    "interface Dot11Radio0",
    "no shutdown",
    f"channel {data['aironetConfig']['channel']}",
    f"encryption mode ciphers {data['aironetConfig']['encr_mod']}",
    f"dot11 ssid {data['aironetConfig']['ssid']}",
    "authentication open",
    "guest-mode",
    "authentication key-management wpa",
    f"wpa-psk ascii {data['aironetConfig']['wifi_pass']}",
    "exit",
    "exit",
    "write memory"
])

print(output)
conn.disconnect()

with open('show_run_output.txt', 'w') as f:
    f.write(output)

print("\n✅ Configuration completed!")



# autowifi-enhanced.py
import json
from netmiko import ConnectHandler

with open('autoAP-jsn.json') as f:
    data = json.load(f)

device = data['aironetInfo']
config = data['aironetConfig']

conn = ConnectHandler(
    device_type=device['device_type'],
    host=device['host'],
    username=device['username'],
    password=device['password'],
    secret=device['secret']
)
conn.enable()

commands = [
    f"hostname {config['hostname']}",
    "default interface Dot11Radio0",
    "interface Dot11Radio0",
    "no shutdown",
    f"channel {config['channel']}",
    f"encryption mode ciphers {config['encr_mod']}",
    f"ssid {config['ssid']}",
    "authentication open",
    "guest-mode",
    "authentication key-management wpa",
    f"wpa-psk ascii {config['wifi_pass']}",
    "exit",
    "interface Dot11Radio0",
    f"ssid {config['ssid']}",
    "bridge-group 1",
    "exit",
    "interface Dot11Radio1",
    "shutdown",
    "exit",
    "dot11 network-map",
    "end",
    "write memory"
]

output = conn.send_config_set(commands)
print(output)
conn.disconnect()

with open('show_run_output.txt', 'w') as f:
    f.write(output)

print("\n✅ Configuration completed successfully!")



# autowifi-yml.py
import yaml
from netmiko import ConnectHandler

with open('autoAP.yml') as f:
    data = yaml.safe_load(f)

device = data['aironetInfo']
config = data['aironetConfig']

conn = ConnectHandler(**device)
conn.enable()

commands = [
    f"hostname {config['hostname']}",
    "default interface Dot11Radio0",
    "default interface GigabitEthernet0",
    "interface Dot11Radio0",
    "no shutdown",
    f"channel {config['channel']}",
    f"encryption mode ciphers {config['encr_mod']}",
    f"dot11 ssid {config['ssid']}",
    "authentication open",
    "guest-mode",
    "authentication key-management wpa",
    f"wpa-psk ascii {config['wifi_pass']}",
    "exit",
    "exit",
    "write memory"
]

output = conn.send_config_set(commands)
print(output)
conn.disconnect()

with open('show_run_output.txt', 'w') as f:
    f.write(output)

print("\n✅ Configuration completed successfully!")



# autoAP-jsn.json
{
  "aironetInfo": {
    "device_type": "cisco_ios_telnet",
    "host": "10.28.10.3",
    "username": "admin",
    "password": "pass",
    "secret": "pass"
  },
  "aironetConfig": {
    "hostname": "28AAAP",
    "ssid": "28-WifiJSON-AA",
    "authentication": "open",
    "key_management": "wpa",
    "wifi_pass": "C1sc0123",
    "channel": "4",
    "encr_mod": "tkip"
  }
}


# autoAP.yml
aironetInfo:
  device_type: cisco_ios_telnet
  host: 10.28.10.3
  username: admin
  password: pass
  secret: pass

aironetConfig:
  hostname: 28AAAP
  ssid: 28-WifiYAML-AA
  authentication: open
  key_management: wpa
  wifi_pass: C1sc0123
  channel: "4"
  encr_mod: tkip






