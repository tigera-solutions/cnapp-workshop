# Module 5: Configure demo resources

**Goal:** Deploy and configure demo resources.

## Steps

1. Deploy policy tiers and base policies.

    We are going to deploy some policies into policy tier to take advantage of hierarcical policy management.

    ```bash
    kubectl apply -f demo/00-base/tiers.yaml
    ```

    This will add tiers `security`, `platform` and `application` to the cluster.

2. Deploy base policies.

    In order to explicitly allow workloads to connect to the Kubernetes DNS component, we are going to implement a policy that controls such traffic and deploy a pass policy into each created tier.

    ```bash
    kubectl apply -f demo/00-base/allow-kube-dns.yaml
    kubectl apply -f demo/00-base/tiers-pass-policy.yaml
    ```

3. Deploy demo applications.

    ```bash
    # deploy dev app stack and hipstershop app stack
    kubectl apply -f demo/app/dev.yaml
    kubectl apply -f demo/app/hipstershop_v0.3.2.yaml
    ```

4. Deploy compliance reports.

    >The reports will be needed for one of a later lab.

    ```bash
    kubectl apply -f demo/40-compliance-reports/daily-cis-results.yaml
    kubectl apply -f demo/40-compliance-reports/cluster-reports.yaml
    ```

5. Deploy global alerts.

    >The alerts will be explored in a later lab.

    ```bash
    kubectl apply -f demo/50-alerts/globalnetworkset.changed.yaml
    kubectl apply -f demo/50-alerts/unsanctioned.dns.access.yaml
    kubectl apply -f demo/50-alerts/unsanctioned.lateral.access.yaml
    ```

6. upgrade openssl from 1.0 to 1.1

```bash
sudo yum -y update
sudo yum install -y make gcc perl-core pcre-devel wget zlib-devel
wget https://ftp.openssl.org/source/openssl-1.1.1k.tar.gz
sudo tar -xzvf openssl-1.1.1k.tar.gz
cd openssl-1.1.1k
sudo ./config --prefix=/usr --openssldir=/etc/ssl --libdir=lib no-shared zlib-dynamic
sudo make
sudo make install
openssl version && cd ..
```

---
[Next -> Module 6](../modules/use-image-assurance.md)
