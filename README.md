# FRR bug reproducer

## Topology

We have 2 frr containers (frr3 and frr2) connected to each other. They belong to two different ASes.

```text
+------------+                               +------------+
|    frr3    |-- 10.0.0.3 --->--- 10.0.0.2 --|    frr2    |
+------------+                               +------------+
```

## Triggering the bug

```shell
# setup the topology:
docker-compose -f docker-compose.yml up -d

# inject some routes
docker exec frr3 bash -c "ip route add 10.1.0.0/16 dev eth0"

# watch the received routes on frr3 (in another terminal)
docker exec frr2 vtysh -c "show ip bgp"

# check the logs
docker cp frr2:/tmp/frr.log .

# teardown
docker-compose -f docker-compose.yml down -v
```