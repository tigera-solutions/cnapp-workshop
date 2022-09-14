# Module 4: Connect image registry


**Goal:** Add image registry to Calico Cloud management plane.

>The workshop uses [microservices-demo](https://github.com/GoogleCloudPlatform/microservices-demo) images of versions `v0.3.2` and `v0.3.8`. In the self guided workshop setting you will need to build and push those images to your image registry and then adjust demo application manifests to reference your image registry.

**Start the CLI scanner**
========================
1. Download the latest version of the CLI scanner.

Linux
```bash
curl -Lo tigera-scanner https://installer.calicocloud.io/tigera-scanner/v3.14.1-9/image-assurance-scanner-cli-linux-amd64
```

MacOS
```bash
curl -Lo tigera-scanner https://installer.calicocloud.io/tigera-scanner/v3.14.1-9/image-assurance-scanner-cli-darwin-amd64
```

2. Set the executable flag on the binary.

```bash
chmod +x ./tigera-scanner
```

3. Verify the CLI scanner works correctly by running the version command.

```bash
./tigera-scanner version
```

**Pull Images**
========================

1. pull adservice image v0.3.2

```bash
docker image pull tigeralabs.azurecr.io/boutiqueshop-demo/adservice:v0.3.2
```

2. verify the downloaded image.

```bash
docker image list
```

**Scan images**
========================

**Online scan**

```bash
./tigera-scanner scan ubuntu:latest --apiurl https://<my-org>.calicocloud.io --token ezBhbGcetc...
```


**Offline scan**

Scan image locally, do not report results

```bash
./tigera-scanner scan tigeralabs.azurecr.io/boutiqueshop-demo/adservice:v0.3.2
```

Scan images with fail and warn thresholds

```bash
./tigera-scanner scan tigeralabs.azurecr.io/boutiqueshop-demo/adservice:v0.3.2 --fail_threshold 7.0 --warn_threshold 3.9 
```


---
[Next -> Module 5](../modules/configure-demo-resources.md)
