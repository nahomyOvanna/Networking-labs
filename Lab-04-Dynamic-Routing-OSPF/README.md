# 🧪 Lab 04: Implementing Dynamic Routing with OSPF

This lab enhances the static routing setup from Labs 2 and 3 by implementing **OSPF (Open Shortest Path First)**. The goal is to provide automatic route advertisement and dynamic failover in the event of a link failure.

By the end of this lab, **R1** and **R2** will dynamically learn routes through **R3**, and the network will adapt to link changes automatically — no manual reconfiguration needed.

## 📝 Lab Objective

- Replace static routing with dynamic OSPF routing
- Establish neighbor relationships and route exchange between R1, R2, and R3
- Simulate and observe OSPF convergence after link failure
- Restore connectivity and confirm network recovery

---

## 🔹 Step 1: Clean Up Static Routing and Confirm Baseline

On both R1 and R2, the following commands were executed to remove static routing:

```
configure
delete protocols static
commit
save
exit
```
After removal, the routing tables were verified on all three routers using: `show ip route`

![R1 Static Route Removed](r1-satatic-route-01.png)

💬 This snapshot confirms that R1’s static routes have been removed. Only directly connected and local routes remain, providing a clean baseline for OSPF.

![R2 Static Route Removed](r2-no-static-route-02.png)

💬 This snapshot shows that R2 is no longer using static routes. The routing table is limited to local and connected networks, as expected before dynamic routing is configured.

![R3 Routing Table Baseline](r3-roting-table-03.png)

💬 R3’s table also reflects a baseline state with only connected and local routes, ready for dynamic OSPF configuration in upcoming steps.

---

## 🔹 Step 2: Configure OSPF on R1, R2, and R3

![R1 OSPF Configuration](r1-config-04.png)
![R2 OSPF Configuration](r2-config-05.png)
![R3 OSPF Configuration](r3-config-06.png)

💬 These outputs confirm that OSPF has been configured on R1, R2, and R3. Each router advertises its directly connected /30 subnet(s) into OSPF Area 0. R3 participates in both the 10.0.13.0/30 and 10.0.23.0/30 networks, acting as the intermediate router that enables R1 and R2 to dynamically exchange routing information.

---

## 🔹 Step 3: Verify OSPF Neighbor Adjacencies and Learned Routes

![OSPF Neighbor Adjacencies](adjacencies-07.png)

💬 These outputs confirm that OSPF adjacencies have been successfully formed. R1 is now a neighbor to R3, and R2 is also a neighbor to R3. This means the OSPF process is correctly identifying and peering with directly connected routers.

![R1 OSPF Learned Routes](r1-route-08.png)
![R2 OSPF Learned Routes](r2-route-09.png)
![R3 OSPF Routing Table](r3-route-10.png)

💬 These outputs show that R1 and R2 have successfully learned remote routes via OSPF. For example, R1 has dynamically learned the 10.0.23.0/30 network through R3, and R2 has learned 10.0.13.0/30 through R3, proving that OSPF is enabling multi-hop communication without static routes

---

## 🔹 Step 4: Simulate a Link Failure and Observe OSPF Convergence

![R3 Link Failure Simulation](r3-link-failure-11.png)

💬 This command simulates a link failure by administratively shutting down the interface between R3 and R2.

![R3 OSPF Neighbor Down](r3-OSPF-12.png)

💬 This output confirms that the OSPF neighbor relationship between R3 and R2 has been lost as a result of the link failure.

![R1 and R2 Updated Routing Tables After Link Failure](r1-2-routing-13.png)

💬 The routing tables on both R1 and R2 now lack the OSPF-learned route to the opposite router’s subnet. This confirms that OSPF has successfully reconverged and withdrawn the unreachable route following the link failure.

![Ping Failure After Link Down](ping-14.png)

💬 This ping result confirms that end-to-end connectivity was lost following the link failure, demonstrating the role of OSPF in maintaining accurate routing based on network changes.

---

## 🔹 Step 5: Restore the Failed Link and Observe OSPF Recovery

![R3 Link Restoration](r3-restore-15.png)

💬 This output confirms that the previously disabled interface has been re-enabled, restoring the physical link between R3 and R2.

![R2 OSPF Neighbor Re-established](r2-ospf-16.png)

💬 The OSPF neighbor relationship with R3 has been re-established, indicating that the protocol detected the restored link and resumed adjacency.

![OSPF Learned Routes Restored](learned-routes-17.png)

💬 The OSPF-learned routes to the opposite subnets have returned, demonstrating that the network has reconverged after the link restoration.

![Ping Success After Link Restoration](ping-18.png)

💬 Successful ping tests confirm that full connectivity between R1 and R2 has been automatically restored through dynamic routing.

---

## ✅ Summary

This lab extended our previous static routing setup by implementing OSPF (Open Shortest Path First) across a 3-router topology. We configured dynamic routing to replace static routes, verified OSPF neighbor adjacencies, and tested network reconvergence after simulating a link failure between R3 and R2. The lab successfully demonstrated how OSPF can automatically adapt to network changes and maintain routing accuracy without manual intervention.


## 🧠 Concepts Reinforced

- OSPF Configuration Basics: Configuring OSPF using /30 point-to-point networks in area 0.

- Neighbor Adjacency: Understanding DR, BDR, and DROther roles via show ip ospf neighbor.

- OSPF Convergence: How OSPF detects and responds to topology changes.

- Routing Table Behavior: Monitoring how OSPF populates or withdraws routes from the RIB (`show ip route`).

- Troubleshooting OSPF: Identifying reasons for lost connectivity (e.g., misconfigurations, downed links, or missing advertisements).

---

## 🟢 Lab Status

### ✅ Completed Successfully
 R1 and R2 were able to dynamically learn and remove routes via R3 using OSPF. The network correctly responded to simulated failures by recalculating the topology and updating routing tables. All steps have been fully tested, documented with snapshots, and annotated with portfolio-grade comments.




