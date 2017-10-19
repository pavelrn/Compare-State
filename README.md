# Compare-State
Get network device state, run diff against device state versions

This playbook uses NAPALM library to communicate to FortiOS devices. The "tweaked" fortios.py library can get the following FortiOS state data:

- interfaces (MAC @, Speed, ...),
- IPsec tunnels (local gw, remote gw, state)
- interfaces_ip (IP @),
- BGP neighbors,
- OSPF neigbors, 
- routing table

## How to run

Collect state before changes to the network devices are made (backup!)
```
ansible-playbook get-state.yml -e output=backups/today
```

Make changes to the network device (for instance, make FortiOS upgrade)
```
ansible-playbook get-state.yml -e output=backups/later_today
```

Use diff to see what has changed
```
colordiff -au backups/today backups/later_today | less -r
```

## Notes for FortiGate devices

For FortiGate devices with VDOMs specify one device per each VDOM. Please see hosts file example below:

```
mogilev-utm.nb1 ansible_host=mogilev-utm.nb ansible_os=fortios ansible_user=admin fortios_vdom=root
mogilev-utm.nb2 ansible_host=mogilev-utm.nb ansible_os=fortios ansible_user=admin fortios_vdom=BN
```


## Thanks, Ivan!

Idea and source code are taken from https://github.com/ipspace/ansible-examples/blob/master/Compare-State-Snapshots/Script.md.
