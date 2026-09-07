# Network bridges and bonds

## Concepts

A bridge joins two or more network segments into one layer-2 domain. Its member interfaces act as ports, allowing devices on the segments to communicate as though they share the same network.

A bond combines multiple links into one logical interface. The goal may be failover, traffic distribution, or increased aggregate throughput, depending on the selected mode and switch configuration.

Common bond modes:

- `balance-rr` (`0`): sends traffic across ports in sequence.
- `active-backup` (`1`): uses one active port and keeps the others as failover links.
- `balance-xor` (`2`): selects a stable port from source and destination values.
- `broadcast` (`3`): sends every packet through every port.
- `802.3ad` (`4`): uses link aggregation; the connected switch must support and configure LACP.
- `balance-tlb` (`5`): balances outgoing traffic.
- `balance-alb` (`6`): balances outgoing traffic and attempts to balance incoming traffic.

## Inspect logical links

```bash
# Show link state for physical and virtual interfaces
ip -details link show

# Show Linux bridge devices and their member ports
bridge link show

# Show bridge forwarding databases
bridge fdb show

# Inspect bonding state, active port, and link failures
cat /proc/net/bonding/<bond-name>

# Show NetworkManager connection profiles
nmcli connection show

# Show device state and the profile applied to each interface
nmcli device status
```

Do not assign the same layer-3 address to both a controller interface and its member ports. For `802.3ad`, coordinate settings on Linux and the physical switch; a mismatch can cause partial connectivity or loops.
