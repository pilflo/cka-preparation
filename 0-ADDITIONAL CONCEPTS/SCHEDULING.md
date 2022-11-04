# scheduling

## Quick wins

### Dedicate particular workload to a node/set of nodes

1. If the relationship is simple (string match) use a **nodeSelector** on the Workload to match a particular label on the **Node**.
2. If the relationship is more complex, use a **nodeAffinity**.

### Avoid a particular Node

1. Add a **Taint** to the **Node**.
2. If you have to schedule some Workload on a **Tainted Node**, use a **Toleration**

### Inter-pod relationships

1. For inter-pod relationships use **podAffinity** and **podAntiAffinity**
