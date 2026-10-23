# 🌐 Static Website Hosting on AWS with CI/CD

A fully automated static website deployment pipeline — push to GitHub, and your site goes live on AWS in minutes. No manual uploads, no manual cache busting.

Built as part of the **[Build With Olamide](https://www.youtube.com/@BuildWithOlamide/videos)** AWS series, where we go from zero to a real, production-style cloud deployment.

🔗 **Live Site:** [your-cloudfront-or-custom-domain-url-here]
🎥 **Watch the full build:** [link to your YouTube tutorial]

---

## 📖 Overview

This project hosts a static website on **Amazon S3**, served globally through **Amazon CloudFront**, secured with a free SSL certificate from **AWS Certificate Manager (ACM)**, and deployed automatically via **AWS CodePipeline** whenever code is pushed to GitHub.

It's designed to show beginners what a real CI/CD workflow looks like — not just "upload files to a bucket," but an actual pipeline you'd find in a production environment.

---

## 🏗️ Architecture

```
 Developer            GitHub Repo         AWS CodePipeline
 ┌─────────┐          ┌───────────┐       ┌────────────────┐
 │  git push├─────────▶│   main    ├──────▶│  Source Stage   │
 └─────────┘          └───────────┘       │  (CodeStar      │
                                            │   Connection)   │
                                            └───────┬─────────┘
                                                     │
                                            ┌────────▼─────────┐
                                            │   Deploy Stage    │
                                            │  (Sync to S3 +    │
                                            │  Invalidate CDN)  │
                                            └────────┬─────────┘
                                                     │
                          ┌──────────────────────────┼──────────────────────────┐
                          │                          │                          │
                 ┌────────▼────────┐        ┌────────▼────────┐       ┌────────▼────────┐
                 │   Amazon S3      │◀──────▶│  CloudFront CDN  │◀─────▶│  ACM (SSL/TLS)  │
                 │ (Static Hosting) │        │  (Global Cache)  │       │   HTTPS Cert    │
                 └──────────────────┘        └──────────────────┘       └──────────────────┘
                                                       │
                                                ┌──────▼──────┐
                                                │   End User   │
                                                │   (Browser)  │
                                                └──────────────┘
```

**Flow:** Push to GitHub → CodePipeline detects the change → syncs files to S3 → invalidates the CloudFront cache → users instantly see the updated site over HTTPS.

---

## ⚙️ Tech Stack

| Layer | Service / Tool |
|---|---|
| Frontend | HTML, CSS, JavaScript |
| Hosting | Amazon S3 (Static Website Hosting) |
| CDN | Amazon CloudFront |
| SSL/TLS | AWS Certificate Manager (ACM) |
| CI/CD | AWS CodePipeline |
| Source Control | GitHub (via CodeStar Connection) |
| DNS *(optional)* | Amazon Route 53 |

## 🚀 CI/CD Pipeline Stages

1. **Source** — CodePipeline is connected to this GitHub repo via a CodeStar Connection. Any push to `main` triggers the pipeline.
2. **Deploy** — The pipeline syncs the repo contents to the S3 bucket configured for static website hosting.
3. **Cache Invalidation** — A CloudFront invalidation is triggered so users get the latest version immediately instead of a cached copy.

---

## 📂 Project Structure

```
├── index.html
├── about.html
├── contact.html
├── /css
│   └── style.css
├── /js
│   └── script.js
├── /images
└── README.md
```

*(Update this to match your actual file structure.)*

---

## 🛠️ Setup & Deployment Guide

### Prerequisites
- AWS account
- GitHub repository (this one)
- A registered domain *(optional, for custom domain + HTTPS)*

### 1. S3 Bucket Setup
- Create an S3 bucket (bucket name should match your domain if using a custom domain)
- Enable **Static Website Hosting** under bucket properties
- Set `index.html` as the index document
- Apply a bucket policy to allow public read access *(or keep private and use CloudFront Origin Access Control — recommended)*

### 2. ACM Certificate
- Request a public certificate in **us-east-1** (required for CloudFront)
- Validate via DNS (CNAME record) in Route 53 or your DNS provider

### 3. CloudFront Distribution
- Create a distribution with the S3 bucket as the origin
- Attach the ACM certificate for HTTPS
- Set the default root object to `index.html`
- *(Optional)* Add your custom domain as an alternate domain name (CNAME)

### 4. CodePipeline Setup
- Create a new pipeline
- **Source:** Connect to this GitHub repo via CodeStar Connection, branch: `main`
- **Deploy:** Use the "Amazon S3" deploy provider, pointing to your bucket
- *(Optional)* Add a CodeBuild stage before deploy if you want to minify/lint files
- *(Optional)* Add a post-deploy step (e.g., AWS CLI/Lambda) to invalidate the CloudFront cache automatically

### 5. Test
- Push a change to `main`
- Watch the pipeline run in the CodePipeline console
- Refresh your CloudFront/domain URL to confirm the update

---

## 📸 Screenshots

*(Add screenshots of your site + your CodePipeline console showing a successful run)*

---

## 🎥 Tutorial

This project is part of the **Build With OlaToms** AWS series, where cloud concepts are broken down for beginners — no jargon, just hands-on building.

📺 Subscribe: [Build With OlaToms](https://www.youtube.com/@BuildWithOlamide/videos)

---

## 👨‍💻 Author

**Olamide** — AWS Authorized Instructor | Cloud Engineer | IT Trainer
🔗 [@ola_toms](https://www.instagram.com/build_with_olamide/) · [Build With Olamide on YouTube](https://www.youtube.com/@BuildWithOlamide/videos)

---

## 📄 License

This project is open source under the [MIT License](LICENSE).
