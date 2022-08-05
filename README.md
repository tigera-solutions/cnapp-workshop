# Cloud-Native application protection workshop

<img src="img/Image-assurance-mitigating-controls.png" alt="Calico on EKS" width="100%"/>

## Workshop objectives

The intent of this workshop is to educate security and technical teams working with Kubernetes platform how to secure cloud-native applications deployed onto the platform using [Calico Cloud](https://www.tigera.io/tigera-products/calico-cloud/). While there are many capabilities available in [Calico Cloud](https://www.tigera.io/tigera-products/calico-cloud/), this workshop focuses on a subset of specific security features used often by security teams and other types of technical users.

## Use cases

In this workshop we are going to focus on these main use cases:

- **Secure applications at deployment time**, leveraging Image Assurance capabilities to detect image vulnerabilities and use security policies to determine next action.
- **Zero-trust workload security and identity-aware microsegmentation**, using identity-aware security policies to configure zero-trust security for cloud-native applications.
- _[WIP]_ **Run-time threat defense**, leveraging Calico Cloud IDS/IPS and anomaly detection features to detect suspicious behaviors and zero-day threats.

## Join the Slack Channel

[Calico User Group Slack](https://slack.projectcalico.org/) is a great resource to ask any questions about Calico. If you are not a part of this Slack group yet, we highly recommend [joining it](https://slack.projectcalico.org/) to participate in discussions or ask questions. For example, you can ask questions specific to EKS and other managed Kubernetes services in the `#eks-aks-gke-iks` channel.

## Workshop prerequisites

>It is recommended to use your personal AWS account which would have full access to AWS resources. If using a corporate AWS account for the workshop, make sure to check with account administrator to provide you with sufficient permissions to create and manage EKS clusters and Load Balancer resources.

- [Calico Cloud trial account](https://www.calicocloud.io/home)
- AWS account and credentials to manage AWS resources
- Terminal or Command Line console to work with AWS resources and EKS cluster
  - most common environments are Cloud9, Mac OS, Linux, Windows WSL2
- `kubectl`
- `Git`
- `netcat`

>This workshop uses [AWS Cloud9](https://docs.aws.amazon.com/cloud9/latest/user-guide/tutorial.html) instance as a workspace environment. If you're familiar with the tools listed in prerequisites section, feel free to use a workspace environment you are most comfortable with.

## Modules

- [Module 1: Set up workspace environment](./modules/set-up-workspace-environment.md)
- [Module 2: Create EKS cluster](modules/create-eks-cluster.md)
- [Module 3: Join EKS cluster to Calico Cloud](modules/join-eks-to-calico-cloud.md)
- [Module 4: Configure demo resources](modules/configure-demo-resources.md)
- [Module 5: Connect image registry](modules/connect-image-registry.md)
- [Module 6: Use image assurance to secure applications at deployment time](modules/use-image-assurance.md)
- [Module 7: Configure zero-trust and microsegmentation policies](modules/use-zero-trust-microsegmentation.md)
- _[WIP]_[Module 8: Use IDS/IPS and anomaly detection to detect and block threats](modules/use-ids-ips-anomaly-detection.md)

## Cleanup

1. Delete application stack to clean up any `loadbalancer` services.

    ```bash
    kubectl delete -f demo/dev/app.manifests.yaml
    kubectl delete -f https://raw.githubusercontent.com/GoogleCloudPlatform/microservices-demo/master/release/kubernetes-manifests.yaml
    ```

2. Delete EKS cluster.

    ```bash
    eksctl delete cluster --name tigera-workshop
    ```

3. Delete EC2 Key Pair.

    >If you created EC2 KeyPair for the EKS cluster, you can remove it if no longer needed.

    ```bash
    export KEYPAIR_NAME='<set_keypair_name>'
    aws ec2 delete-key-pair --key-name $KEYPAIR_NAME
    ```

4. Delete Cloud9 instance.

    Navigate to `AWS Console` > `Services` > `Cloud9` and remove your workspace environment, e.g. `tigera-workspace`.

5. Delete IAM role created for this workshop.

    ```bash
    # use your local shell to set AWS credentials if needed
    # otherwise skip these two lines and execute commands below
    export AWS_ACCESS_KEY_ID="<your_accesskey_id>"
    export AWS_SECRET_ACCESS_KEY="<your_secretkey>"

    # delete IAM role
    IAM_ROLE='tigera-workshop-admin'
    ADMIN_POLICY_ARN=$(aws iam list-policies --query 'Policies[?PolicyName==`AdministratorAccess`].Arn' --output text)
    aws iam detach-role-policy --role-name $IAM_ROLE --policy-arn $ADMIN_POLICY_ARN
    aws iam remove-role-from-instance-profile --instance-profile-name $IAM_ROLE --role-name $IAM_ROLE
    # if this command fails, you can remove the role via AWS Console once you delete the Cloud9 instance
    aws iam delete-instance-profile --instance-profile-name $IAM_ROLE
    aws iam delete-role --role-name $IAM_ROLE
    ```
