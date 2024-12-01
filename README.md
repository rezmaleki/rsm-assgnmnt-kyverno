# **Kyverno Policy Assignment**
 
Some solutions to look for:
● policy engine (like Kyverno)
● custom mutating admission webhook
Policy tests are welcomed, but examples of applications and their modifications will be
sufficient.





## **Solution**
In this solution, I used Kyverno instead of a custom mutating admission webhook because Kyverno is easier to use, doesn’t need custom code, and works directly with Kubernetes. It’s great for creating resources without extra effort.
I would use a custom mutating admission webhook if I needed very specific changes, had to connect to external systems, or need faster performance for complex tasks. However, this requires more work to build and maintain.

This document provides a Kyverno policy for enforcing zone-aware scheduling using topologySpreadConstraints. It ensures workloads are evenly distributed across zones while addressing edge cases such as existing configurations, StatefulSets, Anti-Affinity rules, and application-specific constraints. Additionally, it includes tests for various scenarios to validate the policy.


---

## **Edge Cases**

### **1. Pods with Predefined `topologySpreadConstraints`(handled by rule 1&2):**
  * **Issues:**
  * **Incomplete Definitions:** Pods with missing fields in `topologySpreadConstraints` can lead to scheduling gaps.
  * **Conflicting Definitions:** Pods with conflicting configurations might violate the enforced policy.
---
  * **Solution:**
  * Use `patchStrategicMerge` to:
  Incomplete Definitions: If Pods have incomplete topologySpreadConstraints, the patchStrategicMerge mutation ensures missing fields are added.
  Different Definitions: Pods with conflicting topologySpreadConstraints are validated and denied if they don’t match the enforced configuration.
   ---
   
### **2. StatefulSets(handled by rule 1)** :
  **Persistent Volume Constraints:**
  StatefulSet Pods rely on Persistent Volumes (PVCs) bound to specific zones. Enforcing zone spread might break functionality due to zone-specific storage binding.

---
* **Key Issues:**
1.	**PVCs and Zone Constraints:**
	Pods tied to a zone-specific PV cannot move to other zones.
2.	**Database Applications:**
	Zone balancing may conflict with application-specific replication or performance needs.
---
* **Strategies:**

	 * **Use Zone-Aware Storage:**  
		Opt for solutions like AWS EFS, GCP Filestore, or Ceph that allow cross-zone volume access.
	 * **Avoid Aggressive Balancing:**  
		Exclude StatefulSets from general balancing policies or apply custom policies.
	 * **Fine-Tune Constraints:**  
		Use PreferredDuringSchedulingIgnoredDuringExecution to soften Anti-Affinity and topologySpreadConstraints.
	 * **Monitor and Test:**  
		Regularly test StatefulSet behavior under enforced policies to identify issues.
  ---
### **3. Anti-Affinity Rules**

* **Issues:**
  * Combining `topologySpreadConstraints` with Anti-Affinity rules can cause scheduling conflicts, leaving pods in a pending state.

* **Solution:**
  * Use softer constraints (`PreferredDuringSchedulingIgnoredDuringExecution`) to reduce conflicts.
  * Test compatibility between Anti-Affinity rules and `topologySpreadConstraints` during policy validation.

 ---
### **4.DaemonSets** (handled by rule 1):
---
### **5.Static or System-Critical Pods** 





## **Recommendations**
### **1. Audit Existing Workloads**
Before enforcing the policy, audit the cluster to identify non-compliant workloads. Use the following command:

```bash
kubectl get pods -o yaml | grep -E "nodeSelector|topologySpreadConstraints|schedulerName"
```
### **2. Using Labels for Exceptions**

To exclude specific workloads from the policy:

1. Add a label to the workload:
   ```yaml
   metadata:
     labels:
       exclude-policy: "true"
   ```




   ---




