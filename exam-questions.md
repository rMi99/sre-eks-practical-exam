# 🧪 Senior SRE / DevOps Practical Exam (AWS EKS Edition)

**Target Experience:** 5+ Years
**Environment:** AWS EKS (Elastic Kubernetes Service)

---

## 🔹 Section 1 – Architecture & Provisioning (15 Marks)
You are tasked with provisioning a new production EKS cluster using an Infrastructure-as-Code tool (Terraform is preferred).

1. **VPC Design:** Briefly explain your subnetting strategy for a highly available EKS cluster (Public vs. Private subnets, NAT Gateways). Where do the worker nodes and control plane ENIs live?
2. **Node Constraints:** Create a managed Node Group configured to only use Spot Instances, but with a fallback to On-Demand if Spot capacity is unavailable.
3. **Add-ons:** How would you ensure the VPC CNI, CoreDNS, and kube-proxy are managed and kept up-to-date programmatically?

---

## 🔹 Section 2 – Advanced Authentication & IAM (IRSA) (15 Marks)
Your application needs to read from an S3 bucket named `company-secure-assets`.

1. Instead of using root credentials or node-level IAM roles, implement **IAM Roles for Service Accounts (IRSA)**.
2. Outline the steps to associate an OIDC provider with the cluster.
3. Provide the YAML for the Kubernetes `ServiceAccount` that links to the AWS IAM Role providing S3 `GetObject` access.

---

## 🔹 Section 3 – GitOps & Helm Deployments (20 Marks)
We are moving away from `kubectl apply`. The team uses **ArgoCD** for deployments.

1. Create a `Helm` chart structure for a microservice named `payment-gateway`.
2. How would you structure your Git repository or ArgoCD `Application` manifest to promote changes from `staging` to `production` using GitOps principles?
3. **Challenge:** Configure an ArgoCD Sync Policy that automatically self-heals if someone manually tampers with a live resource via `kubectl`.

---

## 🔹 Section 4 – Advanced Networking & Ingress (15 Marks)
1. Deploy the **AWS Load Balancer Controller**.
2. Write an `Ingress` resource that provisions an **Application Load Balancer (ALB)**, terminates SSL using an ACM Certificate, and routes traffic for `api.company.com` to the `payment-gateway` service.
3. **ExternalDNS:** Explain how you would automate the Route53 record creation for this Ingress.

---

## 🔹 Section 5 – Security & Policy Enforcement (10 Marks)
You need to restrict what developers can deploy.

1. Implement a **NetworkPolicy** that ensures the `database` pods only accept ingress traffic from the `backend` pods, and denies all other internal traffic.
2. Using **Kyverno** or **OPA Gatekeeper**, describe a policy you would enforce to prevent any pod from running as `root` or using the `latest` image tag.

---

## 🔹 Section 6 – Observability & Autoscaling (10 Marks)
1. Configure **Karpenter** (or Cluster Autoscaler). Why might Karpenter be preferred over the traditional Kubernetes Cluster Autoscaler in AWS?
2. Deploy **Prometheus & Grafana**. Write a PromQL query that would trigger an alert if the error rate (HTTP 5xx) of `payment-gateway` exceeds 5% over a 5-minute window.

---

## 🔹 Section 7 – The "3 AM Nightmare" Troubleshooting Task (15 Marks)
**Scenario:** You get paged at 3:00 AM. The production Node.js application is experiencing intermittent connection timeouts to its Aurora RDS database. 

* `kubectl get pods` shows everything is `Running`.
* CPU and Memory on the worker nodes are well below 50%.
* You suspect an underlying EKS network or DNS issue.

**Tasks:**
1. How do you troubleshoot the `kube-dns`/`CoreDNS` performance? What specific logs or metrics would you look at?
2. You discover SNAT port exhaustion on your NAT Gateway. How do you reconfigure the VPC CNI or cluster architecture to mitigate this going forward?
