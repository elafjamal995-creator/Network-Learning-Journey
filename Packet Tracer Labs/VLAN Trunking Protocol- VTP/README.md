############### VTP Config Steps ###############

1. Draw a suitable topology, decorate and comment as necessary.
2. Identify and Configure trunk links between the switches.
3. In regard to VTP, choose one switch to be the VTP server while others become VTP Clients.
4. Configure the VTP server switch with VTP domain, mode, password, and version.
5. Configure the VTP client switches with VTP domain, mode, password, and version.
6. Configure VLANs in only VTP server switch, save the configuration.
7. Go to each VTP client switch to verify if the VLANs have been copied there automatically.
-----------------------------------------------------------------------------------------------


To exchange VTP messages, five requirements must be met:
1. a switch has to be configured as either a VTP server or a VTP client
2. the VTP domain name has to be the same on both switches
3. if present, the VTP domain password has to be the same
4. VTP versions have to match
5. the link between the switches has to be a trunk link
