# EC2 + NGINX + Custom Domain Setup

This project documents the steps I took to set up an **NGINX web server on AWS EC2** and connect it to a **custom domain** via Cloudflare / Route53.  

---

## 1. Buy a Domain
- Registered a domain using **AWS Route53 → Registered domains**.
- Example domain: `aftabn10.co.uk`
*can also be done via Cloudflare*

📸 *Screenshot: Domain registration page*

---

## 2. Launch an EC2 Instance
- Logged into **AWS Console → EC2 → Launch Instances**.
- Configurations:
  - **AMI**: Ubuntu 22.04
  - **Instance type**: `t2.micro` (Free Tier)
  - **Security group**: Allowed inbound traffic on:
    - Port 22 (SSH)
    - Port 80 (HTTP)
- Generated and downloaded a key pair for SSH.

📸 *Screenshot: EC2 instance running in AWS Console*  
📸 *Screenshot: Security group rules (showing ports 22 + 80 open)*  

---

## 3. Install and Start NGINX
Connected via SSH:
```bash
ssh -i my-key.pem ubuntu@<EC2_PUBLIC_IP>
```

Install and enabled NGINX
```bash
sudo apt update -y
sudo apt install nginx -y
sudo systemctl enable nginx
sudo systemctl start nginx
```
Verified it works
```bash
systemctl status nginx
```
📸 *Screenshot: NGINX service status*

Tested in browser:
```cpp
http://<EC2_PUBLIC_IP>
```
*📸 Screenshot: NGINX welcome page via public IP*

## 4. Configure DNS in Route53

- Went to Route53 → Hosted zones.
- Selected the hosted zone for my domain (myname.co.uk).
- Created an A Record:
-Record name: nginx

Record type: A

Value: <EC2_PUBLIC_IP>

Routing policy: Simple

TTL: Default (300 seconds)

📸 Screenshot: Route53 A record configuration

## 5. Test the Domain

Visited:
```
http://nginx.aftabn10.co.uk
```
*📸 Screenshot: Browser showing NGINX page served via domain*

## 6. Stop the Instance 

- After Testing, I stopped the EC2 instance to avoid any compute charges.

## Summary

:heavy_check_mark: Registered domain in Route53<br>
:heavy_check_mark: Deployed EC2 instance with NGINX<br>
:heavy_check_mark: Configured DNS A record in Route53<br>
:heavy_check_mark: Verified NGINX accessible via domain<br>
:heavy_check_mark: Verified NGINX accessible via domain<br>
:heavy_check_mark: Stopped instance to minimize AWS costs