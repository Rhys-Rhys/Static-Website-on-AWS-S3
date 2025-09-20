# 🌐 Static Website Hosting & CI/CD on AWS

## 📌 Project Overview

This project demonstrates how to **host, secure, and automate a static website on AWS**.
It combines multiple AWS services to showcase cloud engineering skills in storage, networking, and DevOps automation.

**Key Features:**

* **Amazon S3** → static website hosting with versioning enabled
* **CloudFront** → CDN for global caching + HTTPS with ACM certificates
* **Route 53** → custom domain integration
* **CodePipeline** → automated CI/CD deployments from GitHub
* **IAM & Policies** → secure access control

---

## 🛠 Architecture

![AWS Static Website CI/CD Architecture](A_flowchart_in_digital_vector_graphic_format_illus.png)

1. Developer pushes code changes to **GitHub**.
2. **CodePipeline** detects commit → builds artifact.
3. Artifact deployed to **Amazon S3** static site bucket.
4. **CodeBuild** (optional) runs CloudFront invalidation.
5. **CloudFront** serves cached content over HTTPS globally.
6. **Route 53** maps custom domain to CloudFront distribution.

---

## 📂 Repository Structure

```
my-static-site/
│
├── index.html          # Homepage
├── error.html          # Error page
├── css/
│   └── styles.css
├── js/
│   └── app.js
├── buildspec.yml       # Used for CloudFront invalidation (optional)
└── README.md
```

---

## ⚙️ AWS Setup Steps

### 1️⃣ Amazon S3 (Static Hosting + Versioning)

* Create bucket: `my-static-website-<unique>`
* Enable: **Static website hosting**
* Enable: **Bucket versioning** (preserves older file versions)
* Bucket policy for public read:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-static-website-<unique>/*"
    }
  ]
}
```

---

### 2️⃣ CloudFront (CDN + HTTPS)

* Create **CloudFront distribution** with origin = S3 bucket
* Add **ACM SSL certificate** (in `us-east-1`) for HTTPS
* Configure default root object = `index.html`
* Optional: enable **HTTP to HTTPS redirect**

---

### 3️⃣ Route 53 (Custom Domain)

* Register or import domain (e.g., `myportfolio.dev`)
* Create **Alias record** → points domain to CloudFront distribution
* Validation: ACM certificate validated via DNS (CNAME record in Route 53)

---

### 4️⃣ CodePipeline (CI/CD Automation)

* **Source stage**: GitHub (branch = `main`)
* **Deploy stage**: Amazon S3 (deploy artifact to website bucket)
* **Post-deploy (optional)**: CodeBuild runs CloudFront invalidation

**Deploy Stage Example Config:**

* Action: `DeployToS3`
* Provider: `Amazon S3`
* Input artifact: `SourceArtifact`
* Bucket: `my-static-website-<unique>`
* Extract file before deploy: `Yes`

**buildspec.yml (CloudFront invalidation):**

yaml
version: 0.2
phases:
  build:
    commands:
      - aws cloudfront create-invalidation --distribution-id <YOUR_DIST_ID> --paths "/*"
---

### 5️⃣ IAM Roles & Permissions

* CodePipeline service role → `s3:PutObject`, `s3:GetObject`, `s3:DeleteObject` on website bucket
* CodeBuild role (if used) → `cloudfront:CreateInvalidation`

---

## 🚀 Deployment Workflow

1. Edit website locally (e.g., `index.html`)
2. Commit & push to `main` on GitHub
3. CodePipeline triggers automatically
4. Files uploaded to S3, versioned, and served by CloudFront
5. Route 53 domain points users to secure HTTPS site

---

## ✅ Learning Outcomes

* Hands-on practice with **AWS S3 versioning, static hosting, and permissions**
* Configured **CloudFront for caching + HTTPS**
* Integrated **Route 53 custom domain** with CloudFront
* Built a **CI/CD pipeline with CodePipeline + GitHub**
* Managed **IAM policies and least-privilege access**

---

This project demonstrates **end-to-end static website deployment and automation** — a practical, production-ready cloud engineering workflow.

---

## 🛠️ Tech Stack

* **AWS S3**
* **AWS Management Console**
* HTML/CSS

---

## 👤 Author

**Rhys Selby**
Aspiring Cloud Engineer
