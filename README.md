# Dynamic Hooks POC

A simple POC showing how to externally (e.g. from an operator) control lifecycle hooks on pods without recreating them.

## Running

While connected to a Kubernetes cluster, create the base resources:

```bash
kubectl apply -f resources.yaml
```

Once you see all pods running, you can inject the new probes into the configmap to see a change in the behavior:

```bash
kubectl apply -f failing.yaml

# To force the Kubelet to immediately resync the configmap volume
kubectl annotate pods -l app=example trigger=failing
```

Pod should start to fail and they'll be restarted by Kubernetes.

You can restore the original probes by running:

```bash
kubectl apply -f working.yaml

# To force the Kubelet to immediately resync the configmap volume
kubectl annotate pods -l app=example trigger=working
```

## Notes

- On Kubelet refresh times: https://github.com/kubernetes/kubernetes/issues/30189
