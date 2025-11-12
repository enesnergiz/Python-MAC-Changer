# Python-MAC-Değiştirici

Gizlilik ve güvenlik amacıyla ağ arayüzünüzün MAC adresini değiştirmek için basit bir Python aracı.  
*(English: A simple Python tool to change your network interface's MAC address for privacy and security.)*

---

##  Kullanım

1. Terminali aç ve bu dizine git:
   ```bash
   cd Python-MAC-Değiştirici
   ```

2. Komutu çalıştır:
   ```bash
   sudo python3 mac_changer.py -i eth0 -m 00:11:22:33:44:55
   ```

3. Örnek çıktı:
   ```
   MyMacChanger started!
   [+] Changing MAC address of eth0 to 00:11:22:33:44:55
   [+] Success! New MAC address: 00:11:22:33:44:55
   ```

---

##  Kod

```python
import subprocess
import optparse
import re

def get_user_input():
    parse_object = optparse.OptionParser()
    parse_object.add_option("-i", "--interface", dest="interface", help="Değiştirilecek arayüz!")
    parse_object.add_option("-m", "--mac", dest="mac_address", help="Yeni MAC adresi")
    (user_input, arguments) = parse_object.parse_args()

    if not user_input.interface:
        parse_object.error("Please specify an interface, use --help for more info.")
    elif not user_input.mac_address:
        parse_object.error("Please specify a MAC address, use --help for more info.")

    return user_input

def change_mac_address(user_interface, user_mac_address):
    print(f"[+] Changing MAC address of {user_interface} to {user_mac_address}")
    subprocess.call(["ifconfig", user_interface, "down"])
    subprocess.call(["ifconfig", user_interface, "hw", "ether", user_mac_address])
    subprocess.call(["ifconfig", user_interface, "up"])

def control_new_mac(interface):
    ifconfig = subprocess.check_output(["ifconfig", interface]).decode("utf-8")
    new_mac = re.search(r"\w\w:\w\w:\w\w:\w\w:\w\w:\w\w", ifconfig)
    if new_mac:
        return new_mac.group(0)
    else:
        return None

print("MyMacChanger started!")

user_input = get_user_input()
change_mac_address(user_input.interface, user_input.mac_address)

finalized_mac = control_new_mac(user_input.interface)

if finalized_mac == user_input.mac_address:
    print(f"[+] Success! New MAC address: {finalized_mac}")
else:
    print("[-] Error: MAC address did not change.")
```

---

##  Uyarılar
- Bu script sadece **Linux** sistemlerde test edilmiştir.  
- Komutları **root (sudo)** yetkisiyle çalıştırmanız gerekir.  
- Yalnızca **etik (ethical hacking)** ve **öğrenme amaçlı** kullanılmalıdır.  

---

##  Yazar
**Enes Nergiz**  
Bilgisayar Mühendisliği Öğrencisi | Siber Güvenlik ve Ağ Meraklısı  
 İletişim: [energiz2310@gmail.com]  
 GitHub: [https://github.com/enesnergiz](https://github.com/enesnergiz)

---

##  Etiketler
`python` `network` `mac-changer` `cybersecurity` `linux` `ethical-hacking`

