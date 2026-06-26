SECURING ALL UNUSED SWITCHPORTS ##

1. Draw necessary topology, decorate and comment
2. Create valid VLANs and name them.
3. Create a blackhole/fake VLAN and name it.
4. Identify all usable ports and assign them respective valid VLANs.
5. Identify all unused ports, assign them the blackhole VLAN and shutdown all of them.
6. Identify and configure trunk links then deny the blackhole VLAN through it.
7. Display trunk information using the command 'sh interface trunk'
8. Try to connect Hacker PCs to any unused port.
