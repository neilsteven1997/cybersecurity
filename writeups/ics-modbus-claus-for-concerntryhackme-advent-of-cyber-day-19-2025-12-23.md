# ICS/Modbus - Claus for Concern  

--- 
The compromise of the TBFC Drone Delivery System illustrates a sophisticated logic manipulation attack against Industrial 
Control Systems (ICS). By interfacing directly with the system's Modbus TCP port (502), the threat actor, identified as 
King Malhare's Eggsploit team, successfully altered physical warehouse operations without triggering standard software-level 
alerts. The attack exploited the inherent lack of authentication and encryption in the Modbus protocol, a legacy industrial 
standard that prioritizes operational reliability over cybersecurity. By manipulating specific holding registers and coils, 
the attackers redirected holiday gift inventory to be replaced by Easter-themed items, effectively sabotaging the delivery 
lifecycle through industrial automation.

The architecture of the TBFC system relies on a SCADA framework where Programmable Logic Controllers (PLCs) act as the 
primary decision-making components. These PLCs interface with physical sensors and actuators to manage package selection 
and drone routing. In this incident, the attackers bypassed the high-level monitoring interfaces and directly modified 
the PLC's internal memory map. Investigation of the Modbus register map revealed that Holding Register 0 (HR0) had been 
set to a value of 1, forcing the system into "Chocolate Egg" selection mode. Furthermore, the system signature in HR4 was 
changed to 666, a clear indicator of the "Eggsploit" framework's presence.

A critical aspect of this breach was the implementation of a logic trap designed to hinder remediation efforts. The attackers 
enabled the protection coil (C11) and linked it to a self-destruct mechanism (C15). This defensive configuration ensures 
that any direct attempt to revert HR0 to its default state while C11 is active triggers an emergency dump protocol (C12), 
rerouting all inventory to the ocean (Zone 10). To evade detection during the compromise, the attackers also disabled audit 
logging (C13) and inventory verification (C10), allowing the system to operate blindly and without a recorded history of 
the malicious configuration changes.

Safe restoration of the drone fleet requires a specific sequence of Modbus commands to disarm the trap before correcting 
the inventory logic. Remediation must begin by writing a "False" value to Coil 11 to disable the protection override. Once 
the monitoring trap is deactivated, HR0 can be safely returned to 0 for Christmas gift selection. Following the logical 
correction, the investigator should re-enable inventory verification (C10) and audit logging (C13) to restore the system's 
integrity and monitoring capabilities. Successful remediation is confirmed when the Christmas Restored Flag (C14) is 
automatically set by the system logic.

---
| Description | Code/Command |
| --- | --- |
| Scan for open Modbus, HTTP, and SSH ports | `nmap -sV -p 22,80,502 <MACHINE_IP>` |
| Import Modbus TCP client in Python | `from pymodbus.client import ModbusTcpClient` |
| Establish connection to PLC | `client = ModbusTcpClient('<MACHINE_IP>', port=502)` |
| Read current package type (HR0) | `client.read_holding_registers(address=0, count=1, slave=1)` |
| Check if protection trap (C11) is active | `client.read_coils(address=11, count=1, slave=1)` |
| Check for Eggsploit signature (HR4) | `client.read_holding_registers(address=4, count=1, slave=1)` |
| Disable protection mechanism (Disarm trap) | `client.write_coil(address=11, value=False, slave=1)` |
| Restore package type to Christmas gifts | `client.write_register(address=0, value=0, slave=1)` |
| Re-enable inventory verification | `client.write_coil(address=10, value=True, slave=1)` |
| Re-enable system audit logging | `client.write_coil(address=13, value=True, slave=1)` |

---
What port is commonly used by Modbus TCP?
- `502`

What's the flag?
- `THM{eGgMas0V3r}`

---
### Key Takeaways - SCADA systems serve as the centralized command centers bridging human operators and industrial machinery.
* PLCs are specialized industrial brains designed for real-time hardware interaction in harsh physical environments.
* The Modbus protocol lacks native authentication, allowing any network-adjacent user to read or write to device registers.
* Coils represent Boolean (On/Off) flags, while Holding Registers store numerical configuration data.
* Attackers in ICS environments often weaponize "protection" logic to create traps that trigger self-destruct or dump
  protocols during unauthorized remediation.
* Always disable protection or override coils (like C11) before attempting to revert manipulated holding registers in a
  compromised PLC.
* Visual monitoring feeds, such as warehouse CCTV, provide essential real-time feedback when verifying the success of
  industrial logic remediation.
