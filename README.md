# ☁ AWS Cloud Portfolio Website
A production-grade static portfolio website deployed entirely on AWS.
**Live at: https://sedoufutoh.com**
---

## 🏗 Architecture
```
Visitor Browser
↓
Route 53 (DNS Resolution)
↓
CloudFront (SSL/TLS · WAF · Global CDN · 400+ edge locations)
↓ Origin Access Control (OAC)
S3 Private Bucket (Origin — index.html)
↓
Portfolio Website Rendered in Browser
```

---
## ☁ AWS Services Used
| Service | Role |
|---------|------|
| Amazon S3 | Private origin storage — hosts index.html |
| Amazon CloudFront | Global CDN — serves from 400+ edge locations |
| AWS Certificate Manager (ACM) | Free SSL/TLS certificate — enables HTTPS
|
| Amazon Route 53 | Domain registration and DNS routing |
| AWS IAM + OAC | Least privilege — S3 accessible only via CloudFront |
| AWS WAF | Web Application Firewall — common vulnerability protection |
---

## 🔒 Security Architecture
- S3 bucket is **completely private** — Block Public Access enabled at bucket and account level
- CloudFront connects to S3 using **Origin Access Control (OAC)** — most secure method
- WAF protects against SQL injection, XSS, and known threat IPs
- All traffic forced to **HTTPS** via viewer protocol policy
- IAM follows **principle of least privilege** throughout
---

## 📋 Why This Project Was Built
As a cloud professional transitioning into the technology industry, I needed a professional online presence visible to recruiters worldwide. This project solved that problem while demonstrating five core AWS services working together —
storage, content delivery, DNS, security, and access control.
---
## 🛠 Step-by-Step Deployment
### Phase 1 — S3 Bucket Setup
1. Create S3 bucket named www.sedoufutoh.com in us-east-1
2. Upload index.html
3. Enable static website hosting
   
### Phase 2 — Domain Registration (Route 53)
4. Register sedoufutoh.com via Route 53
5. Verify domain via email confirmation
   
### Phase 3 — SSL Certificate (ACM)
6. Request public certificate in us-east-1 (required for CloudFront)
7. Add DNS validation records to Route 53
8. Wait for certificate status to show Issued
   
### Phase 4 — CloudFront Distribution
9. Create distribution pointing to S3 bucket
10. Attach ACM certificate
11. Set Default root object to index.html
12. Enable WAF protection
13. Set viewer protocol policy to Redirect HTTP to HTTPS
    
### Phase 5 — Origin Access Control (OAC)
14. Change origin from website endpoint to bucket endpoint
15. Create OAC named sedoufutoh-oac
16. Copy OAC bucket policy to S3
17. Enable Block Public Access on S3 bucket
    
### Phase 6 — Route 53 DNS Records
18. Create A record (root domain) aliased to CloudFront distribution
19. Create CNAME record for www subdomain
20. Verify global DNS propagation at dnschecker.org
---

## ⚡ Challenges and Resolutions
**Challenge — Access Denied after uploading updated index.html**
- Cause: CloudFront Default root object was set to index1.html but the file in S3 was named index.html — a one character mismatch
- - Fix: Updated Default root object in CloudFront General settings to index.html and invalidated cache with /*
- Lesson: Always verify the Default root object exactly matches the filename in S3 after every upload
---

## 💡 Key Lessons Learned
- OAC is the recommended secure method for connecting CloudFront to S3
- ACM certificates must be in us-east-1 regardless of other resource regions
- Always test updates in incognito mode to bypass browser cache
- CloudFront cache invalidation with /* clears all cached files immediately
- DNS propagation takes 15-30 minutes — verify at dnschecker.org
---

## 💰 Monthly Cost
| Resource | Cost |
|----------|------|
| Amazon S3 | ~$0.01/month |
| Amazon CloudFront | Free Tier |
| AWS Certificate Manager | $0 — free forever |
| Route 53 Hosted Zone | $0.50/month |
| Domain (sedoufutoh.com) | ~$1.25/month ($15/year) |
| **Total** | **~$1.76/month** |
---

## ✅ Verification Results
| Test | Result |
|------|--------|
| https://sedoufutoh.com loads | ✅ PASS |
| https://www.sedoufutoh.com loads | ✅ PASS |
| HTTPS padlock visible | ✅ PASS |
| HTTP redirects to HTTPS | ✅ PASS |
| Global DNS propagation | ✅ PASS |
| S3 direct URL returns Access Denied | ✅ PASS |
---

**Built by Sedou Futoh · Calgary, Canada**
LinkedIn: https://www.linkedin.com/in/sedoufutoh
