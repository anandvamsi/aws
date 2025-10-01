What strategies do you employ for team management and mentoring in a DevSecOps environment?
Can you provide an example of how you've implemented security best practices in your cloud environments?
How have you utilized CI/CD pipelines in your previous roles to enhance deployment processes?
Can you describe your experience managing multi-account AWS environments and ensuring compliance?



## What VPC peering can't solve
```bash
VPC peering only provides point-to-point connectivity. It doesn’t support transitive routing, overlapping CIDRs,
or scalable hub-and-spoke designs. For larger networks, Transit Gateway or PrivateLink are better solutions.
That’s why AWS introduced Transit Gateway and PrivateLink for more complex multi-VPC or hybrid network designs
```
## When EKS Fargate makes sense
```bash
Serverless Operations / No Node Management
Small to Medium Workloads
Bursty or Unpredictable Workloads
Multi-Tenant Clusters / Strong Isolation Needs
Dev/Test Environments
Tighter Security Boundaries
```

- When to use Helm vs Kustomize
```bash
Native Kubernetes Integration
Kustomize is built into kubectl (kubectl apply -k).
No extra tool or client needed, unlike Helm which requires the Helm CLI.
Pure Declarative YAML (No Templating Language)
Kustomize works with standard YAML + overlays.
Helm introduces a templating language (Go templates) → adds complexity and logic that’s not native YAML.
No Hidden State / Release Metadata
Helm maintains state in Kubernetes (via ConfigMaps/Secrets in the cluster).
Kustomize is stateless → what’s in Git is exactly what gets applied. This is cleaner for GitOps workflows.
Easy Environment Overlays Without Duplication
Kustomize overlays let you apply small patches to a base config.
With Helm, managing multiple environments means separate values.yaml files, which can get messy to maintain.
Better GitOps Alignment
Tools like ArgoCD and Flux natively support Kustomize without extra plugins.
Helm often needs additional controllers (e.g., Helm Operator) to fully integrate with GitOps.

Security & Simplicity

Kustomize applies transformations only — no templating engine, no client-side scripting, no Tiller-like component (old Helm v2 issue).

This reduces the attack surfac




```
- Why GitOps pull beats push in prod

## When to use initContainers vs lifecycle hooks
```bash
Init containers are special containers in Kubernetes that run before app containers.
They’re used for setup tasks like waiting for dependencies, running migrations, or preparing configuration.
They always run to completion before the main containers start
```

## Usecases
```bash
Wait for dependencies (e.g., ensure a DB is up before app starts).
Configuration setup (e.g., fetch config files from S3 before starting the app).
Data migration / schema check (e.g., run DB migrations before the app runs).
Permission setup (e.g., set volume ownership/permissions before main app uses it).
```
```bash
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
spec:
  initContainers:
  - name: init-db
    image: busybox
    command: ['sh', '-c', 'until nslookup mydb; do echo waiting for db; sleep 2; done']
  containers:
  - name: myapp
    image: nginx
    ports:
    - containerPort: 80
```

## What is the difference between init containers and PostStart and PreStop
```bash
Init containers are best when you need to prepare the environment before the main app runs — for example, fetching secrets, waiting for dependencies, or setting permissions. 
Lifecycle hooks (postStart, preStop) are tied to the app container itself, so they’re better for startup/shutdown logic like warming caches or graceful termination.
In short:
Init containers = environment setup before app start
Lifecycle hooks = container-specific actions during start/stop.”
```


- What nodeSelector vs affinity actually control
- When to use spot over on demand in production
- Why DaemonSet is not a good fit for scaling workers


- P1
- 1. How would you design a scalable, highly available CI/CD system for microservices across multiple teams?
- 2. How would you manage cross-region deployments using Terraform in a multi-cloud setup?
- 3. How do you implement GitOps in a Kubernetes environment?
- 4. Can you explain how you would create a fully automated blue-green deployment in a Kubernetes-based microservices architecture?
- 5. How do you design an end-to-end DevSecOps pipeline for a fintech application with strict compliance requirements (e.g., PCI-DSS)?

-p2
- 6. What are some best practices for managing pipeline as code in large, distributed teams?
- 7. How would you dynamically provision ephemeral environments (dev/test) using pipelines?
- 8. In a monorepo setup, how do you ensure that only relevant services are built and deployed in a CI/CD pipeline?
- 9. How do you implement a canary deployment strategy with real-time monitoring rollback in a CI/CD system?
- 10. How do you manage secrets and config securely at scale in Kubernetes without compromising GitOps workflows?


-p3
- 11. Explain the control plane components of Kubernetes and how you would harden them for production use.
- 12. How would you scale a Kubernetes cluster horizontally across multiple regions and still ensure zero-downtime upgrades?
- 13. What is a PodDisruptionBudget and how do you use it in critical workloads?
- 14. How do you implement and manage network policies in Kubernetes for strict inter-service communication?
- 15. How would you refactor a legacy Terraform codebase used by multiple teams to follow best practices like DRY and modularity?
- 16. Explain the internals of how Terraform handles dependencies and graph building during the planning phase.

-p3
17. How do you manage and isolate Terraform state files across multiple environments and teams?
18. What’s your strategy to prevent and recover from a corrupted or deleted remote backend state file?
19. Have you implemented policy-as-code (e.g., Sentinel, OPA) with Terraform? Give a real use case.
20. How would you implement a centralized logging solution across multiple cloud platforms and environments?
21. What’s your approach to securing cloud-native DevOps infrastructure with Identity Federation (e.g., Azure AD + AWS IAM)?

-p4
22. How do you set up workload identity federation between GitHub Actions and Google Cloud / Azure securely?
23. How do you ensure cost-efficient auto-scaling of infrastructure in cloud when managing high workloads in CI/CD?
24. Explain a scenario where you had to design a disaster recovery (DR) strategy for DevOps infrastructure.
25. How do you enforce compliance and auditability in your CI/CD processes across global regions (e.g., GDPR, HIPAA)?
26. What’s your strategy for managing container image security across all stages of a DevOps pipeline?
27. How would you integrate runtime threat detection in Kubernetes using tools like Falco or Sysdig?






DevOps Architect Interview at Atlassian 

Round 1 – Infra, Kubernetes, and Cloud Patterns (45 mins)
• Design a multi-tenant EKS cluster with isolation across dev, QA, and prod, with no noisy neighbors.
• What’s your approach to managing 10+ Kustomize overlays without drift or duplication?
• Explain how you’d secure cross-region S3 replication and validate data integrity at scale.
• What happens when systemd hits a failing unit in a containerized node? How would you auto-recover?
• Walk through your strategy to detect & mitigate pod-to-pod lateral movement inside a cluster.
• How do you perform zero-downtime upgrades for a stateful workload using Helm 3?
• Describe a hybrid cloud routing architecture between GCP and AWS. Where do you enforce boundaries?
• Your Terraform state got corrupted during a backend migration. Rebuild strategy?
• Bash One-liner: Find all running containers using more than 500MB RSS memory on a node.

Round 2 – Real Fire, RCA, and Chaos Control (75 mins)
• A new AWS ALB config caused TLS handshakes to fail intermittently. Walk through your full RCA path.
• Kubernetes nodes are healthy. But kubectl logs is blank for critical pods. What’s happening?
• You deployed a sidecar logging agent. Suddenly, CPU throttling spikes. Diagnose and rollback.
• Autoscaling isn’t kicking in despite the CPU crossing the threshold. What’s broken — metrics, HPA, or API server?
• Prod users reporting 504s, but ELB health checks are green. Explain your isolation + triage process.
• Systemd journal logs vanish on reboot across some AMIs. What do you check in the image build and boot sequence?
• A production pod was OOMKilled, but you can’t find logs. Walk through a forensic-level debug.
• Kernel panic on a GKE node mid-deploy. How do you identify if it’s infra, base image, or app-level?

Round 3 – Leadership, Engineering Influence & Production Principles (30 mins)
• How do you design infrastructure that empowers devs without giving them footguns?
• What’s your Linux-level checklist before approving any custom AMI to production?
• You’ve been asked to move from centralized logging to a service-mesh-based observability model. Your tradeoffs?
• Describe how you simulate production-level chaos in staging for Kubernetes.
• How do you handle pushback from leadership when your SLOs threaten velocity?
