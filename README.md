# terraform-aws-eks-argocd

[![Lint Status](https://github.com/DNXLabs/terraform-aws-eks-argocd/workflows/Lint/badge.svg)](https://github.com/DNXLabs/terraform-aws-eks-argocd/actions)
[![LICENSE](https://img.shields.io/github/license/DNXLabs/terraform-aws-eks-argocd)](https://github.com/DNXLabs/terraform-aws-eks-argocd/blob/master/LICENSE)


Terraform module for deploying Kubernetes [Argo CD](https://argoproj.github.io/argo-cd/) and [Argo Rollouts](https://argoproj.github.io/argo-rollouts/) inside a pre-existing EKS cluster.

## Usage

```bash
module "argocd" {
  source = "git::https://github.com/DNXLabs/terraform-aws-eks-argocd.git?ref=0.1.0"

  enabled = true
}
```

#### Login

The initial password is autogenerated with the pod name of the ArgoCD API server:
```bash
ARGO_PWD=`kubectl get pods -n argocd -l app.kubernetes.io/name=argocd-server -o name | cut -d'/' -f 2`
```

In order to access the Argo CD server URL, we are going to use the kubectl port-forward command to access the application.
```bash
kubectl port-forward --address 0.0.0.0 -n argocd service/argo-cd-argocd-server 8001:443
```

export ARGOCD_SERVER=`kubectl get svc argocd-server -n argocd -o json | jq --raw-output .status.loadBalancer.ingress[0].hostname`

Using admin as login and the autogenerated password:
```bash
argocd login localhost:8001 --username admin --password $ARGO_PWD --insecure
```

You should get as an output:
```bash
'admin' logged in successfully
```

<!--- BEGIN_TF_DOCS --->
<!--- END_TF_DOCS --->

## Authors

Module managed by [DNX Solutions](https://github.com/DNXLabs).

## License

Apache 2 Licensed. See [LICENSE](https://github.com/DNXLabs/terraform-aws-eks-argocd/blob/master/LICENSE) for full details.