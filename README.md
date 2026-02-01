# 🌐 Amazon S3 Static Website Hosting Project

## 📌 Overview
This project demonstrates hosting a static website using **Amazon S3**.  
It was created as a hands-on exercise to learn cloud storage, bucket policies, logging, lifecycle rules, and static site hosting.  
It also doubles as an **exam-friendly study guide** with AWS CLI commands for each step.

access the website from below url 
http://s3-website-hosting-2026.s3-website-us-east-1.amazonaws.com

## 🧠 Key Idea (Exam Gold)
- Amazon S3 can host static websites without EC2, Load Balancers, or web servers.  
- This makes it **cheap, scalable, highly available, and serverless**.  
- A static website contains HTML, CSS, JavaScript, images, and documents, and does not require server-side runtimes.

---

## 🚀 Step-by-Step Implementation

### 1️⃣ Prepare Website Files (Local Machine)
- Downloaded a static website template (e.g., from tooplate.com).
- Typical files:
  - `index.html` (main entry point)
  - CSS files
  - JavaScript files
  - Images and assets

**Exam Tip:** The entry file must be `index.html`.


### 2️⃣ Create the Main S3 Bucket
Rules:
- Bucket name must be globally unique and lowercase.
- Versioning is optional but powerful.

**AWS CLI:**
```bash
aws s3api create-bucket --bucket barista908 --region us-east-1
aws s3api put-bucket-versioning --bucket barista908 --versioning-configuration Status=Enabled
```

**Why Versioning?**
- Protects against accidental deletion.
- Allows rollback to previous versions.
- Increases storage cost (important exam point).

---

### 3️⃣ Upload Website Files
**AWS CLI:**
```bash
aws s3 sync ./website-files s3://barista908
```

**Exam Tip:** By default, all S3 objects are **private** after upload.

---

### 4️⃣ Create a Second Bucket for Access Logs
Best practice: store logs in a separate bucket.

**AWS CLI:**
```bash
aws s3api create-bucket --bucket barista908accesslogs --region us-east-1
```

**Why separate?**
- Security
- Auditing
- Log isolation

---

### 5️⃣ Make Website Objects Public
Steps:
1. Disable Block Public Access  
2. Allow ACLs (Object Ownership)  
3. Make objects public  

**AWS CLI:**
```bash
aws s3api put-public-access-block --bucket barista908 \
--public-access-block-configuration BlockPublicAcls=false,IgnorePublicAcls=false,BlockPublicPolicy=false,RestrictPublicBuckets=false

aws s3api put-bucket-ownership-controls --bucket barista908 \
--ownership-controls Rules=[{ObjectOwnership=ObjectWriter}]

aws s3api put-object-acl --bucket barista908 --key index.html --acl public-read
```

**Exam Tip:** Every new object upload is private by default.

---

### 6️⃣ Enable Static Website Hosting
Configuration:
- Index document: `index.html`
- Error document: `error.html`

**AWS CLI:**
```bash
aws s3 website s3://barista908 --index-document index.html --error-document error.html
```

**Result:** S3 generates a **website endpoint URL**, different from the object URL.

---

### 7️⃣ Enable Server Access Logging
Tracks:
- Who accessed the website
- Browser type
- Time and request details

**AWS CLI:**
```bash
aws s3api put-bucket-logging --bucket barista908 \
--bucket-logging-status '{"LoggingEnabled":{"TargetBucket":"barista908accesslogs","TargetPrefix":"logs/"}}'
```

**Exam Point:** AWS automatically creates a bucket policy on the logs bucket to allow S3 to write logs.

---

### 8️⃣ Access the Website
Use the **S3 Static Website Endpoint**, not the object URL.

**Example:**
```
http://barista908.s3-website-us-east-1.amazonaws.com
```

✅ No EC2  
✅ No Apache  
✅ No Load Balancer  
✅ Fully serverless

---

### 9️⃣ Understand S3 Versioning
Versioning:
- Keeps every version of an object.
- Prevents permanent deletion by default.
- Adds a Delete Marker instead of deleting data.

**AWS CLI:**
```bash
aws s3api list-object-versions --bucket barista908
```

---

### 🔟 Deleting Objects in a Versioned Bucket
- **Normal Delete:** Adds a Delete Marker (data still exists).  
- **Permanent Delete:** Must delete each version explicitly.

**AWS CLI:**
```bash
# Normal delete (adds delete marker)
aws s3 rm s3://barista908/ABOUT_THIS_TEMPLATE.txt

# Permanent delete (specific version)
aws s3api delete-object --bucket barista908 --key ABOUT_THIS_TEMPLATE.txt --version-id <version-id>
```

---

## 📷 Architecture Diagram
```
[User Browser] ---> [Amazon S3 Bucket] ---> [Static Website Hosting Endpoint]
```

---

## 📚 Learning Outcomes
- Hosting static websites using AWS S3.
- Writing and applying bucket policies.
- Enabling server access logging for audits.
- Managing storage with lifecycle rules.
- Performing cleanup and deletion safely.
- Documenting cloud projects professionally on GitHub.

---

## 🛠 Technologies Used
- **Amazon S3**
- **AWS IAM Policies**
- **HTML, CSS, JavaScript**


✅ This README now combines:
- **Hands-on project documentation** (for recruiters).  
- **Exam-friendly notes with AWS CLI commands** (for study).  
- **Architecture + references** (for completeness).  

access the website from below url 

http://s3-website-hosting-2026.s3-website-us-east-1.amazonaws.com
