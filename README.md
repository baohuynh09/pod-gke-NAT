### Problem
- pod GKE can not connect to VPN network


### Reason
- Pod GKE is not using NAT when going out of GKE worker


### Solution
- Using a daemonSet with "Priviledge: true" toi check & apply NAT on Iptables for every 60s
```bash
iptables -A POSTROUTING -d 10.60.0.0/16 -m comment --    comment "NAT-VPN: SNAT for outbound traffic through VPN" -m addrtype ! --  dst-type LOCAL -j MASQUERADE -t nat
``` 


### Verification
- ssh to GKE workers
```bash
- "iptables-save | grep NAT" => you'll see NAT rules added
```
- In the pod, now you can reach VPN network (10.60.0.0/16)


Reference: https://blog.mrtrustor.net/post/iptables-kubernetes/
