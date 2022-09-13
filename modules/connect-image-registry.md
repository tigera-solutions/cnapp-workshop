# Module 4: Connect image registry


**Goal:** Add image registry to Calico Cloud management plane.

>In the instructor-led workshop, you will be provided with the credentials to access the Azure Container Registry. When following this workshop in a self guided setting, you will need to configure and connect your own container registry.  
The workshop uses [microservices-demo](https://github.com/GoogleCloudPlatform/microservices-demo) images of versions `v0.3.2` and `v0.3.8`. In the self guided workshop setting you will need to build and push those images to your image registry and then adjust demo application manifests to reference your image registry.

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


**Scan images**
========================



---
[Next -> Module 5](../modules/configure-demo-resources.md)
