# three-router-nat-configure-basicSimulation
so how to do is
firstly to make like me is using 3 pt routers and 3 pt swtich connected, and 6 pcs connected to swtitcs
then u need add manually IP address to the pcs
example : PC0/1 is 10.10.10.0/24 and PC2/3 is 20.20.20.0/24, PC4/5 is 30.30.30.0/24
then u need add tho the ips on the switch and router.
the swtich u need adding the pc gateway
example IF PC0/1 ip is 10.10.10.0, then i might be the gateway is 10.10.10.0 (this what i used to be).
and for the router u need adding the address with same host network like
router 0 is 40.40.40.1 connected to 40.40.40.2 at router 1 and router 1 connected to router 2 so same like 0 to 1
then just look the picture
