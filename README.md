# Python-MAC-Changer
A simple Python tool to change the MAC address of your network interface for privacy and security purposes. (Türkçesi: Ağ arayüzünüzün MAC adresini gizlilik ve güvenlik amacıyla değiştiren basit bir Python aracı.)

import subprocess
import optparse
import re

def get_user_input():
    parse_object = optparse.OptionParser()
    parse_object.add_option("-i", "--interface", dest="interface", help="Interface to change!")
    parse_object.add_option("-m", "--mac", dest="mac_address", help="New MAC address")
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
