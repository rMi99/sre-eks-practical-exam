# 📘 How to Use This Exam

This repository contains a **Senior SRE / DevOps Practical Exam** tailored for AWS EKS environments, designed to assess a candidate's practical skills and problem-solving abilities.

## 🎯 Purpose
This exam goes beyond basic Kubernetes concepts and evaluates expertise in:
- High Availability and Cloud Architecture
- Security and Identity Management (IAM, IRSA, Network Policies)
- GitOps implementation with tools like ArgoCD
- Advanced Networking (Ingress, AWS Load Balancer Controller)
- Observability (Prometheus, Grafana)
- High-stakes Troubleshooting (The "3 AM Nightmare")

## 🚀 Conducting the Exam

### For Interviewers:
1. **Provide the Scenario:** Share the `exam-questions.md` file with the candidate.
2. **Environment Setup:** Have a sandbox AWS account or an empty cluster ready if you want them to write and execute actual code (e.g., Terraform or YAML manifests). 
3. **Time Limit:** We recommend a timebox of 2 to 3 hours for the practical components, followed by a 1-hour interview to review architecture, choices, and reasoning.
4. **Interactive Troubleshooting:** Section 7 is best conducted as an interactive roleplay. Present the scenario, answer their investigatory questions, and observe their deductive reasoning.

### For Candidates:
1. **Fork/Clone:** Treat this like a real project. Set up your repository structure.
2. **Implement Solutions:** Write your Terraform code for Section 1, and Helm/Kubernetes manifests for the subsequent sections. Save your configuration files in appropriately named directories.
3. **Document Your Thought Process:** For questions requiring explanations (e.g., subnetting strategy, troubleshooting steps), write down your answers in a `solutions.md` file or comment extensively in your code.
4. **Focus on Best Practices:** Remember that you are being evaluated for a Senior position. Ensure your configurations reflect industry standards for security, scalability, and maintainability.

## 🛡️ Best Practices & Evaluation Criteria

When evaluating the solutions, focus on these critical areas:

* **Infrastructure as Code (IaC):** Is the Terraform state modular? Are variables used correctly? Is the syntax clean?
* **Security First (Least Privilege):** Are IAM policies strictly scoped? Is IRSA correctly implemented instead of generic worker node roles?
* **Resilience:** Do configurations account for high availability? (e.g., PodDisruptionBudgets, proper NodeGroup strategies).
* **GitOps Alignment:** Does the proposed ArgoCD setup facilitate automated, declarative deployments and prevent configuration drift?
* **Clear Communication:** Can the candidate explicitly convey their reasoning, especially during the troubleshooting phase? Is their documentation clear?
