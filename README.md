# myk8s

A **very opinionated** chart for initial Kubernetes setup

Ideal for local k8s clusters (like minikube or docker-for-mac kubernetes).
It setups a suite of handy tools to help you operate the cluster.
Like I said, it's very opinionated, so you don't need to care about details.
It **ignores** security measures completely (like TLS or authentication), therefore you should not use this is production.

It installs:
- nginx-ingress
- kubernetes-dashboard
- docker-registry
- grafana, prometheus and alertmanager
- jenkins

An ingress exits exposing the following endpoints (the root domain can be changed):
- alertmanager.myk8s
- dashboard.myk8s
- grafana.myk8s
- prometheus.myk8s
- registry.myk8s
- jenkins.myk8s

Prometheus collects metrics and grafana shows metrics for:
- k8s cluster nodes and pods

# Requirements
- kubernetes cluster running
- helm

# Install
```
./scripts/install
```

myk8s setups a DNS service (coredns) that is resolving the ".myk8s" domain and exposed on port 32000 of your host.
To be able to your host resolve ".myk8s" you need to let him know about this new DNS server.

A easy way to do it is:
```
sudo bash -c 'echo -e "nameserver 127.0.0.1\nport 32000" > /etc/resolver/myk8s'
```

# Uninstall
```
./scripts/uninstall
sudo rm -rf /etc/resolver/myk8s
```

# Why using the 'scripts/install' instead of just "helm install" the myk8s chart?
Having a big chart with big and complex dependencies will become later harder to maintain. Helm doesn't provide an easy way of managing dependencies independently. Therefore it was opt to install/update/delete each of the dependencies

Also, for better readability, is nice to have values split in several files by application, something that helm doesn't allow.

Maybe Helm 3 can fix some of this issues.
