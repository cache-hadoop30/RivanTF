autowifi-jsn.py

  import json
from netmiko import ConnectHandler

data = json.load(open('autoAP-jsn.json'))

conn = ConnectHandler(**data['aironetInfo'])
conn.enable()

output = conn.send_config_set([
    f"hostname {data['aironetConfig']['hostname']}",
    "default interface Dot11Radio0",
    "interface Dot11Radio0",
    "no shutdown",
    f"channel {data['aironetConfig']['channel']}",
    f"encryption mode ciphers {data['aironetConfig']['encr-mod']}",
    f"dot11 ssid {data['aironetConfig']['ssid']}",
    "authentication open",
    "guest-mode",
    "authentication key-management wpa",
    f"wpa-psk ascii {data['aironetConfig']['wifi-pass']}",
    "exit",
    "exit",
    "write memory"
])

print(output)
conn.disconnect()

open('show_run_output.txt', 'w').write(output)
print("\n✅ Configuration completed!")
import json
from netmiko import ConnectHandler

# Step 1: Load config
with open('autoAP-jsn.json') as f:
    data = json.load(f)

device = data['aironetInfo']
config = data['aironetConfig']

# Step 2: Connect
conn = ConnectHandler(
    device_type=device['device_type'],
    host=device['host'],
    username=device['username'],
    password=device['password'],
    secret=device['secret']
)
conn.enable()

# Step 3: Send correct config
commands = [
    f"hostname {config['hostname']}",
    "default interface Dot11Radio0",
    "interface Dot11Radio0",
    "no shutdown",
    f"channel {config['channel']}",
    f"encryption mode ciphers {config['encr-mod']}",
    f"ssid {config['ssid']}",
    "authentication open",
    "guest-mode",
    "authentication key-management wpa",
    f"wpa-psk ascii {config['wifi-pass']}",
    "exit",  # exit SSID mode
    "interface Dot11Radio0",
    f"ssid {config['ssid']}",   # re-bind SSID
    "bridge-group 1",
    "exit",  # exit Dot11Radio0
    "interface Dot11Radio1",    # if second radio exists, shutdown it
    "shutdown",
    "exit",
    "dot11 network-map",        # <<< THIS is needed to broadcast
    "end",
    "write memory"
]

output = conn.send_config_set(commands)
print(output)

# Step 4: Disconnect
conn.disconnect()

# Step 5: Save output
with open('show_run_output.txt', 'w') as f:
    f.write(output)

print("\n Configuration completed successfully!")


  autowififi-tf.py
  import json
from netmiko import ConnectHandler

with open('autoAP-jsn.json') as f:
    data = json.load(f)

conn = ConnectHandler(
    device_type=data['aironetInfo']['device_type'],
    host=data['aironetInfo']['host'],
    username=data['aironetInfo']['username'],
    password=data['aironetInfo']['password'],
    secret=data['aironetInfo']['secret']
)

conn.enable()

commands = [
    f"hostname {data['aironetConfig']['hostname']}",
    "default interface Dot11Radio0",
    "interface Dot11Radio0",
    "no shutdown",
    f"channel {data['aironetConfig']['channel']}",
    f"encryption mode ciphers {data['aironetConfig']['encr-mod']}",
    f"dot11 ssid {data['aironetConfig']['ssid']}",
    "authentication open",
    "guest-mode",
    "authentication key-management wpa",
    f"wpa-psk ascii {data['aironetConfig']['wifi-pass']}",
    "exit",
    "exit",
    "write memory"
]

output = conn.send_config_set(commands)
print(output)

conn.disconnect()

with open('show_run_output.txt', 'w') as f:
    f.write(output)

autowifi-yml.py
  import yaml
from netmiko import ConnectHandler

# Load config
with open('autoAP.yml') as f:
    data = yaml.safe_load(f)

device = data['aironetInfo']
config = data['aironetConfig']

# Connect
conn = ConnectHandler(**device)
conn.enable()

# Configure
commands = [
    f"hostname {config['hostname']}",
    "default interface Dot11Radio0",
    "default interface GigabitEthernet0",
    "interface Dot11Radio0",
    "no shutdown",
    f"channel {config['channel']}",
    f"encryption mode ciphers {config['encr-mod']}",
    f"dot11 ssid {config['ssid']}",
    "authentication open",
    "guest-mode",
    "authentication key-management wpa",
    f"wpa-psk ascii {config['wifi-pass']}",
    "exit",
    "exit",
    "write memory"
]

output = conn.send_config_set(commands)
print(output)

# Disconnect
conn.disconnect()

# Save output
with open('show_run_output.txt', 'w') as f:
    f.write(output)

print("\n Configuration completed successfully!")

