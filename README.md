import re

# File path
file_path = '/mnt/data/log.txt'

# Read the file and count unique IP addresses
with open(file_path, 'r') as file:
    log_data = file.read()

# Regular expression for IP addresses
ip_pattern = re.compile(r'\b(?:[0-9]{1,3}\.){3}[0-9]{1,3}\b')
ips = ip_pattern.findall(log_data)
unique_ips = set(ips)

# Count of unique IPs
len(unique_ips)
