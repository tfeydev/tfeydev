# 👋 Welcome

🎓 Currently learning **Java** and **Spring Boot** through Chad Darby's Udemy courses.  
💡 Interests: **backend development**, **REST APIs**, **full‑stack applications**.  
🌱 Goal: Progress from Java fundamentals to building professional Spring Boot apps.

## 📚 Courses in Progress
- *Java Programming for Beginners* (Udemy)  
- *Java Masterclass – Become a Java Developer* (Udemy)  
- *Spring Boot 3, Spring 6 & Hibernate for Beginners* (Udemy)

## 📌 Course Repositories
- 🔗 [fromJava2SpringBoot](https://github.com/tfeydev/fromJava2SpringBoot)  
- 🔗 *(more to be added)*

## 🛠️ Tech Stack
`Java` `Spring Boot` `Maven` `React` `Angular` `PostgreSQL` `Docker` `RHEL 10 Linux`

---

📎 LinkedIn: [linkedin.com/in/thorstenfey](https://linkedin.com/in/thorstenfey)  
📂 All projects: [github.com/tfeydev](https://github.com/tfeydev)

---

## 📂 Project Overview

This repository serves as a central hub linking to related projects and sub‑repositories:

- [ConnectionTestOciJavaSdk](./ConnectionTestOciJavaSdk) – Java SDK connection tests for Oracle Cloud Infrastructure.  
- [containers](./containers) – Container configurations and Dockerfiles.  
- [linux](./linux) – Linux scripts, configurations, and guides.  
- [oracle](./oracle) – Oracle‑specific scripts and setup files.  
- [scripts](./scripts) – Utility scripts for various purposes.

> *Note:* Some folders may be `.gitignore`d and not pushed to GitHub, but remain locally for development.

---

# SOP: Deploying a RHEL Image to OCI with SSH/SFTP Access

1. **Prepare prerequisites**  
   - Access to Red Hat Insights (Image Builder) and Oracle Cloud Infrastructure (OCI) tenancy  
   - A VCN/subnet with inbound TCP/22 allowed  
   - SSH client available on the local system  

2. **Build image in Red Hat Insights**  
   - Navigate to `console.redhat.com/insights/image-builder`  
   - Configure RHEL release, packages, and settings  
   - Generate/upload SSH key pair (public key embedded for default cloud user)  
   - Build image; note format (e.g., `.qcow2`)  

3. **Import image into OCI**  
   - Transfer via direct URL import or download/upload to OCI Object Storage  
   - In OCI: *Compute → Custom Images → Import image*  
   - Set source to Object Storage, format to QCOW2, import and wait for “Available” status  

4. **Launch VM from custom image**  
   - *Compute → Instances → Create instance*  
   - Select imported image, desired shape, target subnet  
   - Add SSH public key for an existing VM user (e.g., `cloud-user`)  
   - Ensure a public IP is assigned; launch instance  

5. **Configure SSH for intended user**  
   - Determine actual login user in VM (`cloud-user` or custom)  
   - Locally derive public key:  
     ```bash
     ssh-keygen -y -f /path/to/private_key
     ```  
   - In VM (via Console Connection if needed):  
     ```bash
     mkdir -p ~/.ssh
     chmod 700 ~/.ssh
     echo "PUBLIC_KEY" >> ~/.ssh/authorized_keys
     chmod 600 ~/.ssh/authorized_keys
     ```  

6. **Verify SSH access**  
    ```bash
    ssh -i /path/to/private_key <username>@<public_IP>
    ```

7. **Configure SFTP in FileZilla**
    - Protocol: SFTP
    - Host: <public_IP>
    - Port: 22
    - User: <username>
    - Key file: /path/to/private_key

8. **Optimize storage**
    - If no further instances are needed from the custom image, delete it from OCI to avoid storage fees.
    - Optionally export the image to Object Storage and download locally before deletion.
