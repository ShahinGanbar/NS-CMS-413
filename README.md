# Network Security Final Preparation

## Hostname and Interface Description Configuration

### Hostname Configuration

```bash
Router(config)# hostname <name>
```

### Interface Description Configuration

```bash
Router(config)# interface gigabitEthernet 0/0
Router(config-if)# description Sw1
```

---

# IP Planning

- **Network ID:** All bits to the right of the subnet bits must be `0`.
- **Broadcast ID:** All bits to the right of the subnet bits must be `1`.
- **First Usable IP:** Network ID + 1
- **Last Usable IP:** Broadcast ID - 1
- **Total Usable Hosts:** `2^(32 - subnet_bits) - 2`

---

## PC Configuration

Navigate to:

```text
Desktop → IP Configuration
```

Assign:

- IPv4 Address
- Subnet Mask
- Default Gateway

---

## Router IP Configuration

```bash
Router(config)# interface gigabitEthernet 0/0
Router(config-if)# ip address <ip-address> <subnet-mask>
Router(config-if)# no shutdown
```

---

## Switch Management IP Configuration

```bash
Switch(config)# interface vlan 1
Switch(config-if)# ip address <ip-address> <subnet-mask>
Switch(config-if)# no shutdown
```

---

# STP PortFast

Enable PortFast on access ports:

```bash
Switch(config)# interface range fastEthernet 0/x-y
Switch(config-if)# spanning-tree portfast
```

---

# STP BPDU Guard

Enable BPDU Guard on access ports:

```bash
Switch(config)# interface range fastEthernet 0/x-y
Switch(config-if)# spanning-tree bpduguard enable
```

---

# DHCP Snooping

Enable DHCP Snooping for VLAN 1 and trust the uplink port:

```bash
Switch(config)# ip dhcp snooping vlan 1

Switch(config)# interface gigabitEthernet 0/1
Switch(config-if)# ip dhcp snooping trust

Switch(config-if)# end
Switch# show ip dhcp snooping
```

---

# SSH Configuration

> Hostname must be configured before generating RSA keys.

```bash
R1(config)# ip domain name exam.com
R1(config)# username admin password admin

R1(config)# crypto key generate rsa
How many bits in the modulus [512]: 1024

R1(config)# ip ssh version 2

R1(config)# line vty 0 15
R1(config-line)# transport input ssh
R1(config-line)# login local
R1(config-line)# exit

R1(config)# enable password admin
```

---

# ACL Configuration

## Allow SSH from a Specific Host

```bash
R1(config)# ip access-list extended 100

R1(config-ext-nacl)# permit tcp host 192.168.10.1 host 192.168.10.254 eq 22
R1(config-ext-nacl)# exit

R1(config)# interface gigabitEthernet 0/0
R1(config-if)# ip access-group 100 in
R1(config-if)# exit
```

---

## Allow ICMP (Ping) to a Specific Host

```bash
R1(config)# ip access-list extended 100

R1(config-ext-nacl)# permit icmp 192.168.10.0 0.0.0.255 host 192.168.10.254 echo
```

---

# Useful Verification Commands

## DHCP Snooping

```bash
show ip dhcp snooping
```

## SSH

```bash
show ip ssh
show crypto key mypubkey rsa
```

## ACL

```bash
show access-lists
show ip interface
```

## STP

```bash
show spanning-tree
```

## Interface Status

```bash
show ip interface brief
```
