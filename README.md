findstr /R "[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}" arquivo_de_log.txt | sort | uniq -c


import re
from collections import Counter

# Abre o arquivo de log
with open('arquivo_de_log.txt', 'r') as file:
    logs = file.read()

# Expressão regular para encontrar IPs no arquivo de log
ip_pattern = re.compile(r'\d+\.\d+\.\d+\.\d+')
ips = ip_pattern.findall(logs)

# Conta a frequência de cada IP
ip_count = Counter(ips)

# Imprime os IPs e suas respectivas contagens
for ip, count in ip_count.items():
    print(f'{ip}: {count}')
