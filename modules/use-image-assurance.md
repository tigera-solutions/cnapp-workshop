# Module 6: Use image assurance to secure application deployment process

**Goal:** Configure Calico Cloud Admission Controller to secure application deployment process.

1. Configure the Admission Controller.

    Calico Cloud uses the Admission Controller to accept or reject resources that create pods based on configured `ContainerAdmissionPolicies` rules. For more information refer to [Calico Cloud Admission Controller](https://docs.calicocloud.io/image-assurance/install-the-admission-controller) documentation.

    Configure the Admission Controller.

    ```bash
    # create workdir
    mkdir admission-controller-install && cd admission-controller-install
    # generate certs
    export URL="https://installer.calicocloud.io/manifests/v3.14.1-9/manifests" && curl ${URL}/generate-open-ssl-key-cert-pair.sh | bash
    # generate admission controller manifest
    export URL="https://installer.calicocloud.io/manifests/v3.14.1-9/manifests" && \
    export IN_NAMESPACE_SELECTOR_KEY="apply-container-policies" && \
    export IN_NAMESPACE_SELECTOR_VALUES="true" && \
    curl ${URL}/install-ia-admission-controller.sh | bash
    # install admission controller
    kubectl apply -f ./tigera-image-assurance-admission-controller-deploy.yaml
    ```

    >The Admission Controller only watches the namespaces it is configured to track. You can configure namespace label via `IN_NAMESPACE_SELECTOR_KEY` and `IN_NAMESPACE_SELECTOR_VALUES` variables used in the commands above. Explore `tigera-image-assurance-admission-controller-deploy.yaml` manifest to see how those values are configured.

    Add label to the application namespace to allow the Admission Controller to watch it.

    ```bash
    kubectl label namespace default apply-container-policies=true
    ```

2. Configure container admission policies.

    The `ContainerAdmissionPolicies` resources are used to configure policies for Admission Controller.

    Deploy container policies.

    ```bash
    kubectl apply -f demo/10-container-admission-policy/reject-failed.yaml
    ```

3. Deploy application to test container admission policy.

    ```bash
    # delete hipstershop app stack
    kubectl delete -f demo/app/hipstershop_v0.3.2.yaml --grace-period=0
    # redeploy hipstershop app stack
    kubectl apply -f demo/app/hipstershop_v0.3.2.yaml
    ```

    >Several images should have `Fail` scanning result and as a result container admission policy block the deployment of failed images.

4. Configure vulnerability exceptions.

    In cases where vulnerability cannot be immediately addressed and/or determined to be a low risk, one may configure exception for it to exclude it from scanning status result and allow the image to be deployed.

    Navigate to `Image Assurance` > `Scan Resultss` view and select any image with `Fail` status of version `v0.3.8`, then select all `High` and `Critical` vulnerabilities in the right side panel and create exception for them. One can create exception for all occurrences of these vulnerabilities across all images, or for specific image repository, or even specific image version.

    Configure vulnerability exceptions for all `v0.3.8` images to allow them get a passing score.

---
[Next -> Module 7](../modules/use-zero-trust-microsegmentation.md)
