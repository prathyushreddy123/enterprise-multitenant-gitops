# 🚀 Enterprise Multi-Tenant GitOps with ArgoCD, Helm & ApplicationSet

Welcome to the `enterprise-multitenant-gitops` repository — a **production-grade GitOps architecture** designed to manage **multi-tenant Kubernetes environments** using **ArgoCD**, **Helm**, and **ApplicationSet**. This project is based on the [Google microservices-demo](https://github.com/GoogleCloudPlatform/microservices-demo) and is tailored to demonstrate real-world DevOps workflows suitable for large-scale deployments.

---

## 🌟 Why This Project?

This repo is built as part of a hands-on learning journey to:

Understand GitOps principles in a scalable, enterprise-grade setup

Implement ArgoCD with multi-tenant support

Master Helm chart reuse across microservices

Automate ArgoCD Applications using ApplicationSet

Prepare for real-world DevOps interviews and enterprise use cases

📦 Why Use the Microservices Demo?

The Google microservices-demo is a real-world e-commerce app with 10+ services, making it a perfect candidate to simulate:

Service-to-service communication

Namespace isolation

Continuous delivery of independent workloads

This project started by creating a separate repo for the source code and CI/CD workflows to build Docker images. Once built, the images are:

Pushed to a container registry (e.g., DockerHub or GCR)

Then this GitOps repo consumes those images via Helm values (tagged versions)

So, this repo is completely focused on infrastructure deployment via GitOps — it does not contain source code, only:

Helm templates

Environment-specific values

ArgoCD Application definitions

ApplicationSet logic

Shared platform components (RBAC, NetworkPolicy, etc.)

By separating source code from deployment infrastructure, this setup mirrors real-world enterprise DevOps practices (GitOps + CI separation).

---

## 🧱 Tech Stack

| Tool              | Purpose                                |
| ----------------- | -------------------------------------- |
| 🌀 ArgoCD         | GitOps Controller                      |
| 🛠 Helm           | Application packaging and templating   |
| 📦 Kustomize      | Infra overlays (namespace, rbac, etc.) |
| 📁 ApplicationSet | Dynamically generate ArgoCD apps       |
| 🔒 SealedSecrets  | Encrypted secret management            |

---

## 🏗️ Directory Structure

```
enterprise-multitenant-gitops/
├── apps/                                # Helm chart templates for all microservices
│   └── microservices-demo/
│       └── charts/                      # Shared templates (deployment, service, ingress, etc.)
│           ├── templates/
│           └── values.yaml              # Default values (disabled by default)
│
├── tenants/
│   └── microservices-team/
│       ├── overlays/
│       │   └── dev/                     # Microservice-specific Helm values for dev
│       │       ├── values-frontend.yaml
│       │       ├── values-adservice.yaml
│       │       └── ...
│       │
│       └── helm-release-dev/           # ArgoCD app definitions (one-time) + ApplicationSet
│           └── applicationset.yaml     # ApplicationSet to dynamically generate apps per microservice
│
├── clusters/
│   └── dev/
│       └── apps.yaml                   # App of Apps — deploys common infra & ApplicationSet
│
├── common/                             # Kustomize overlays for shared infra
│   └── overlays/dev/                   # Namespace, RBAC, Istio, NetworkPolicy, etc.
│
├── secrets/                            # Encrypted SealedSecrets or SOPS-based secrets
│
└── tools/argocd/                       # ArgoCD bootstrap configurations (projects, access, etc.)

```

---

## 🔁 GitOps Flow Using ApplicationSet

1. All microservices use the **same Helm chart templates** under `apps/microservices-demo/charts/`
2. Each microservice has its own **Helm values YAML** under `tenants/microservices-team/overlays/dev/`
3. An **ApplicationSet** reads these files and **auto-generates ArgoCD Applications** dynamically
4. Each ArgoCD Application deploys one microservice, independently managed
5. **Changes to values YAML** = auto sync to cluster 🔄

---

## ✨ Features

- ✅ Multi-tenant support (via overlays and namespaces)
- ✅ Centralized Helm chart with reusable templates
- ✅ Fully automated ArgoCD app creation using ApplicationSet
- ✅ Support for dev/prod environments (easily extendable)
- ✅ Secrets managed using SealedSecrets
- ✅ CI/CD-ready directory layout

---

## 📦 Supported Microservices

- `frontend`
- `adservice`
- `cartservice`
- `checkoutservice`
- `currencyservice`
- `emailservice`
- `productcatalogservice`
- `recommendationservice`
- `shippingservice`
- `loadgenerator`
- `paymentservice`

Each service:

- Uses shared chart templates
- Is deployed by an ArgoCD Application (auto-generated)
- Has environment-specific values under `tenants/.../overlays/dev`

---

## 🧪 How to Apply

```bash
# 1. Deploy the ApplicationSet
kubectl apply -f tenants/microservices-team/helm-release-dev/applicationset.yaml -n argocd

# 2. (Optional) Bootstrap using apps.yaml
kubectl apply -f clusters/dev/apps.yaml -n argocd

# 3. Watch ArgoCD Applications get created dynamically
kubectl get applications -n argocd
```

---

## 💬 About the Author

Hi! I'm **Prathyush**, a DevOps & Cloud Engineer passionate about building production-grade platforms with automation and scalability in mind. This project is part of my deep dive into enterprise GitOps, designed both for **personal learning**.

---

## 📌 Contributions / Questions

This repo is actively being developed. Feel free to:

- Create issues or discussions
- Fork and adapt for your org
- Suggest improvements via PR

---

## 🔗 Links

- [Microservices Demo App](https://github.com/GoogleCloudPlatform/microservices-demo)
- [ArgoCD](https://argo-cd.readthedocs.io/)
- [ApplicationSet Docs](https://argo-cd.readthedocs.io/en/stable/operator-manual/applicationset/)
- [Helm](https://helm.sh/)
- [Kustomize](https://kubectl.docs.kubernetes.io/references/kustomize/)
- [Kubeseal](https://github.com/bitnami-labs/sealed-secrets)

---

## 👏 Special Thanks

Thanks to the Kubernetes community, ArgoCD maintainers, and projects like Stakater & Google’s microservices-demo for the ecosystem support and reference architectures.

---

*This repo reflects enterprise GitOps design principles and aims to serve as both a practical learning project and a professional showcase.*