# Nova-App: Manual Deployment (Template)

`AWS` · `Kubernetes` · `Docker` · `Node.js`

[![AWS](https://img.shields.io/badge/AWS-EKS-orange)](https://aws.amazon.com/eks/)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?logo=kubernetes&logoColor=white)](https://kubernetes.io/)
[![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)](https://www.docker.com/)
[![Node.js](https://img.shields.io/badge/Node.js-339933?logo=node.js&logoColor=white)](https://nodejs.org/)


This is the starting point for my two-part article series on deploying a Node.js app to Amazon EKS with a CI/CD pipeline. It is the Nova-App project exactly as it was after the **manual deployment** from my [first article](https://medium.com/@gloriaboakye/deploying-a-simple-node-js-app-on-aws-eks-a-beginner-and-budget-friendly-guide-02d1e101fd26): an Express app, a Dockerfile and Kubernetes manifests, with no automation yet.
 

Use it to follow along, add the GitHub Actions pipeline yourself, and watch every `git push` deploy to EKS.

## Use This Template

1. Click **Use this template**, then select **Create a new repository**.
2. Name your new repository `nova-app`. Part 1 configures an IAM trust policy expecting `YOUR_GITHUB_USERNAME/nova-app` , if you use a different repository name, update the trust policy to match.
3. Clone your new repository (its default branch is `main`, which your Part 1 workflow will watch):

```bash
# Download your copy of the starter repo
git clone https://github.com/YOUR_GITHUB_USERNAME/nova-app.git

# Move into the project folder
cd nova-app

The default branch of your copy is `main`. That is the branch the pipeline you build in Part 1 watches.

```
 
## What is in this repository
 
```
nova-app/
├── server.js                # Express app with / and /health endpoints
├── package.json             # Dependencies
├── package-lock.json        # Locked dependency versions
├── Dockerfile               # Docker image definition
├── k8s/
│   ├── cluster-config.yaml  # EKS cluster configuration (eksctl)
│   └── deployment.yaml      # Kubernetes Deployment and Service
├── .gitignore
├── LICENSE
└── readme.md
```
 
## What you will add while following Part 1
 
- A `/version` endpoint, so you can see which commit is running
- Jest tests, and a real `npm test` script
- Build arguments in the Dockerfile that carry the commit and build time into the app
- An Amazon ECR repository to hold your images
- An IAM role that GitHub Actions assumes with OIDC, so no AWS keys are stored in GitHub
- Kubernetes RBAC that lets the pipeline update only this one Deployment
- A GitHub Actions workflow, `.github/workflows/deploy.yml`
## Before you start: four things to update
 
This starter is the project as it was before the automation, so a few files need change before they work:
 
- **`k8s/cluster-config.yaml` pins Kubernetes `1.28`.** EKS no longer supports that version, and `eksctl` will refuse it. Part 1 uses `1.34`. Check the EKS Kubernetes versions page on the day you build, and pick a version that is still in **standard support**. Older versions in extended support cost extra every hour.
- **`k8s/deployment.yaml` points at my Docker Hub image** (`glowbak/nova-app:v1`). Replace the `image:` line with the ECR repository URI you create in Part 1.
- **The Dockerfile uses `FROM node:18`.** Part 1 builds on Node 24, so change it to `FROM node:24`.
- **`package.json` has npm's default test script**, which always fails. Part 1 replaces it with Jest.
## Cost warning
 
An EKS cluster costs money for every hour it exists, and the load balancer starts billing as soon as it is created. Delete the cluster when you are done:
 
```bash
# Removes the control plane, nodes and VPC. Your ECR repo and IAM role stay.
eksctl delete cluster --name nova-app-cluster --region us-east-1
```
 
## Prerequisites
 
- [AWS account](https://aws.amazon.com/free/) and the [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html), configured
- [eksctl](https://eksctl.io/installation/) and [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- [Node.js](https://nodejs.org/) and Git
## Related resources
 
- **Part 1 of the series:** [LINK-TO-PART-1]
- **First article (the manual deployment):** [Deploying a Simple Node.js App on AWS EKS](https://medium.com/@gloriaboakye/deploying-a-simple-node-js-app-on-aws-eks-a-beginner-and-budget-friendly-guide-02d1e101fd26)
- **The finished project, with the full code:** [Globak143/nova-app](https://github.com/Globak143/nova-app)
## Author
 
**Gloria Boakye**
 
- LinkedIn: [linkedin.com/in/gloriaboakye](https://linkedin.com/in/gloriaboakye)
- Medium: [@gloriaboakye](https://medium.com/@gloriaboakye)
- GitHub: [github.com/Globak143](https://github.com/Globak143)
## License
 
This project is open source and available under the [MIT License](LICENSE).