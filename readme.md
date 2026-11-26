# Nova-App: Manual Deployment (Template)

[![AWS](https://img.shields.io/badge/AWS-EKS-orange)](https://aws.amazon.com/eks/)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?logo=kubernetes&logoColor=white)](https://kubernetes.io/)
[![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)](https://www.docker.com/)
[![Node.js](https://img.shields.io/badge/Node.js-339933?logo=node.js&logoColor=white)](https://nodejs.org/)


You cannot automate what you haven't done manually first. That is the idea behind this repository. 

Nova-App is a simple Express application I previously deployed by hand to Amazon EKS, and this template gives you that working starting point: the app, Dockerfile, and Kubernetes manifests.

In this [article series](https://medium.com/@gloriaboakye/want-to-deploy-your-app-to-eks-on-every-git-push-8e61321ffed4?postPublishedType=initial), we'll build on that manual deployment by adding a GitHub Actions CI/CD pipeline. You'll configure Amazon ECR, IAM authentication through GitHub OIDC, Kubernetes RBAC and the deployment workflow that brings everything together.

Use this template to follow along, build the pipeline yourself, and turn a manual deployment process into one where every git push can trigger an automated deployment to EKS.


## Use This Template

1. Click **Use this template**, then select **Create a new repository**.

2. Name your new repository `nova-app`. Part 1 configures an IAM trust policy expecting `YOUR_GITHUB_USERNAME/nova-app` , if you use a different repository name, update the trust policy to match.

3. Clone your new repository. The pipeline you build will watch the main branch by default.

```bash
# Download your copy of the starter repo
git clone https://github.com/YOUR_GITHUB_USERNAME/nova-app.git

# Move into the project folder
cd nova-app

```

## Repository Layout
 
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
 
## Part 1 Roadmap
 
- A `/version` endpoint, so you can see which commit is running
- Jest tests, and a real `npm test` script
- Build arguments in the Dockerfile that carry the commit and build time into the app
- An Amazon ECR repository to hold your images
- An IAM role that GitHub Actions assumes with OIDC, so no AWS keys are stored in GitHub
- Kubernetes RBAC that lets the pipeline update only this one Deployment
- A GitHub Actions workflow, `.github/workflows/deploy.yml`

## Baseline Updates
 
I made three small updates to the original manual deployment, so you can start straight away:
 
- **`k8s/cluster-config.yaml`** now pins Kubernetes `1.34`. The original `1.28` is no longer supported by EKS, and `eksctl` refuses it.
- **The Dockerfile** now starts from `node:24`. It used `node:18`.
- **`k8s/deployment.yaml`** now has a placeholder ECR image, `123456789123.dkr.ecr.us-east-1.amazonaws.com/nova-app:latest`. It used my Docker Hub image previously.


## Your Setup Checklist:
 
- **Replace `123456789123` with your AWS account ID** in `k8s/deployment.yaml`. Part 1 shows exactly where.
- **Check the Kubernetes version on the day you build.** Support dates change. Pick a version that is still in **standard support**, because older versions in extended support cost extra every hour.
- **`package.json` still has npm's default test script**, which always fails. Part 1 replaces it with Jest in Phase 2.2.


## Billing & Cleanup

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

## Articles & Reference Links
 
- **Part 1 of the series:** [Want to Deploy Your App to EKS on Every git push](https://medium.com/@gloriaboakye/want-to-deploy-your-app-to-eks-on-every-git-push-8e61321ffed4?postPublishedType=initial)

- **First article (the manual deployment):** [Deploying a Simple Node.js App on AWS EKS](https://medium.com/@gloriaboakye/deploying-a-simple-node-js-app-on-aws-eks-a-beginner-and-budget-friendly-guide-02d1e101fd26)

- **The finished project, with the full code:** [Globak143/nova-app](https://github.com/Globak143/nova-app)
## Author
 
**Gloria Boakye**
 
- LinkedIn: [linkedin.com/in/gloriaboakye](https://linkedin.com/in/gloriaboakye)

- Medium: [@gloriaboakye](https://medium.com/@gloriaboakye)

- GitHub: [github.com/Globak143](https://github.com/Globak143)

## License
 
This project is open source and available under the [MIT License](LICENSE).