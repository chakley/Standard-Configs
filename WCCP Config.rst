WCCP Config
===========

.. code-block:: c

	
	ip wccp 61 redirect-list acl-wccp-61
 
	ip wccp 62 redirect-list acl-wccp-62

	ip access-list extended acl-wccp-61
	 remark traffic from LAN
	 # deny traffic from Riverbed VLAN subnet
	 deny   tcp 10.134.99.16 0.0.0.7 any
	 # deny traffic to Riverbed VLAN subnet
	 deny   tcp any 10.134.99.16 0.0.0.7
	 deny   tcp any host 10.134.21.1
	 deny   tcp any host 10.134.22.1
	 deny   tcp host 10.134.21.1 any
	 deny   tcp host 10.134.22.1 any
	 permit ip 10.134.21.0 0.0.0.255 any
	 permit ip 10.134.22.0 0.0.0.255 any
	 deny   ip any any

	ip access-list extended acl-wccp-62
	 remark Traffic from WAN
	 deny   tcp 10.134.99.16 0.0.0.7 any
	 deny   tcp any 10.134.99.16 0.0.0.7
	 deny   tcp any host 10.134.21.1
	 deny   tcp any host 10.134.22.1
	 deny   tcp host 10.134.21.1 any
	 deny   tcp host 10.134.22.1 any
	 permit ip any 10.134.21.0 0.0.0.255
	 permit ip any 10.134.22.0 0.0.0.255
	 deny   ip any any

	# WAN Interface
	int g0/0/0 
	 ip wccp 62 redirect in

	# LAN Interface
	int g0/0/1 
	 ip wccp 61 redirect in

	# Riverbed Interface
	int g0/0/2 
	 ip wccp redirect exclude in


end
