# Cilium Cheatsheet (Kubernetes Security & Networking)

## **Cilium Overview**
Cilium is an open-source, cloud-native networking and security solution for Kubernetes, providing enhanced observability and security with eBPF.

---

## **Cilium Installation**
```sh
kubectl create namespace kube-system
helm repo add cilium https://helm.cilium.io/
helm repo update
helm install cilium cilium/cilium --namespace kube-system --set hubble.enabled=true
```

### **Verify Installation**
```sh
kubectl get pods -n kube-system | grep cilium  # Check Cilium pods
kubectl get ds -n kube-system | grep cilium   # Check Cilium DaemonSet
cilium status                                 # Check Cilium health
```

---

## **Network Policies**

### **Allow Traffic to a Specific Namespace**
```yaml
apiVersion: cilium.io/v2
kind: CiliumNetworkPolicy
metadata:
  name: allow-namespace
spec:
  endpointSelector:
    matchLabels:
      app: my-app
  ingress:
  - fromEndpoints:
    - matchLabels:
        io.kubernetes.pod.namespace: my-namespace
```
```sh
kubectl apply -f allow-namespace.yaml  # Apply network policy
```

### **Deny All External Traffic**
```yaml
apiVersion: cilium.io/v2
kind: CiliumNetworkPolicy
metadata:
  name: deny-external
spec:
  endpointSelector: {}
  ingress:
  - fromEntities:
    - cluster
```
```sh
kubectl apply -f deny-external.yaml  # Apply deny policy
```

---

## **Hubble Observability**
Hubble provides deep observability for network flows in Cilium.

### **Enable Hubble CLI**
```sh
cilium hubble enable      # Enable Hubble
cilium hubble status      # Check Hubble status
```

### **Monitor Network Traffic**
```sh
hubble observe            # Show all network flows
hubble observe --from-pod my-app  # Show traffic for a specific pod
hubble observe --protocol http    # Filter by HTTP traffic
```

---

## **Cilium CLI Commands**
```sh
cilium status              # Check Cilium status
cilium policy get          # Get active Cilium policies
cilium endpoint list       # List Cilium-managed endpoints
cilium monitor             # Live traffic monitoring
```

---

## **Uninstall Cilium**
```sh
helm uninstall cilium -n kube-system
kubectl delete namespace kube-system
```

---

## **Useful Cilium Resources**
- [Cilium Docs](https://docs.cilium.io/)
- [Cilium Network Policies](https://docs.cilium.io/en/stable/policy/)
- [Hubble Observability](https://docs.cilium.io/en/stable/gettingstarted/hubble/)

---

This cheatsheet provides a quick reference for Cilium installation, network policies, observability, and CLI commands. Let me know if you need any modifications!

