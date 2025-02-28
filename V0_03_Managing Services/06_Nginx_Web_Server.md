# **Nginx Web Server: Installation, Configuration, and Advanced Features**

## **Why Should DevOps and Cloud Engineers Learn Nginx?**

Imagine you are a **DevOps Engineer** managing the deployment of **The EpicBook!**, an online bookstore. Your team notices that under **high traffic conditions**, the website slows down. Users **experience delays** in browsing books and adding items to their carts.

Or, as a **Cloud Engineer**, you are responsible for **scaling the infrastructure** and ensuring **zero downtime**. You need to **distribute user requests across multiple servers**, improving **availability and performance**.

This is where **Nginx** comes in.

By the end of this lesson, you will be able to:
✔ **Understand how Nginx works and why it is different from Apache.**  
✔ **Install and configure Nginx on Ubuntu/Debian and CentOS/RHEL.**  
✔ **Set up server blocks (virtual hosts) for multiple applications.**  
✔ **Configure Nginx as a reverse proxy and load balancer for The EpicBook! backend.**  
✔ **Secure Nginx with HTTPS and IP whitelisting.**

Let’s begin by understanding **how Nginx handles web traffic differently from Apache**.

---

## **1. What is Nginx and Why is it Different from Apache?**

Nginx (pronounced "Engine-X") is a **high-performance, lightweight web server** and **reverse proxy**. Unlike Apache, which follows a **process-driven model**, Nginx uses an **event-driven, asynchronous architecture**, which allows it to:

- **Handle thousands of simultaneous connections** efficiently.  
- **Optimize static content delivery** (e.g., images, CSS, JavaScript).  
- **Act as a reverse proxy** to distribute requests to backend services (e.g., Node.js).  
- **Load balance traffic** across multiple servers.

---

## **2. Real-World Use Case: Hosting The EpicBook! with Nginx**

For **The EpicBook!**, we need to host **two separate applications**:

| Application | URL | Purpose |
|------------|-----------------------------|------------------------------|
| **Main Bookstore** | `http://theepicbooks.com` | Serves the frontend bookstore (static website) |
| **Backend API** | `http://api.theepicbooks.com` | Handles book listings, user accounts, and orders (Node.js + MySQL) |

Nginx will be used to:
✔ **Serve static assets for the frontend**  
✔ **Proxy requests to the backend API**  
✔ **Load balance traffic for high availability**  

---

## **3. Installing Nginx**

### **On Ubuntu/Debian**

#### **Step 1: Update the Package List**
```bash
sudo apt update
```

#### **Step 2: Install Nginx**
```bash
sudo apt install nginx -y
```

#### **Step 3: Start and Enable Nginx**
```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

✅ **Verification Step:**  
```bash
systemctl status nginx
```

---

### **On CentOS/RHEL**

#### **Step 1: Install Nginx**
```bash
sudo yum install nginx -y
```

#### **Step 2: Start and Enable Nginx**
```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

✅ **Verification Step:**  
```bash
systemctl status nginx
```

Now that Nginx is installed, let’s explore **its configuration files**.

---

## **4. Nginx Configuration Basics**

| File Type | Location |
|-----------|----------------------------|
| **Main Configuration File** | `/etc/nginx/nginx.conf` |
| **Server Blocks (Virtual Hosts)** | `/etc/nginx/sites-available/` and `/etc/nginx/sites-enabled/` |
| **Access Log** | `/var/log/nginx/access.log` |
| **Error Log** | `/var/log/nginx/error.log` |

Now, let’s configure **server blocks** to serve **The EpicBook!** and its backend.

---

## **5. Setting Up Server Blocks (Virtual Hosts)**

### **Why Use Server Blocks?**
Nginx **server blocks** allow us to:
- Host **multiple applications** on the same server.
- Assign **separate domains** to different components of The EpicBook!.
- Apply **different configurations** for frontend and backend services.

---

### **Step 1: Create Directories for The EpicBook!**
```bash
sudo mkdir -p /var/www/theepicbooks.com/html
sudo mkdir -p /var/www/api.theepicbooks.com/html
sudo chmod -R 755 /var/www/theepicbooks.com
sudo chmod -R 755 /var/www/api.theepicbooks.com
```

---

### **Step 2: Add Test Files for Both Applications**

#### **Main Bookstore**
```bash
echo "Welcome to The EpicBook!" | sudo tee /var/www/theepicbooks.com/html/index.html
```

#### **Backend API**
```bash
echo "The EpicBook API is running!" | sudo tee /var/www/api.theepicbooks.com/html/index.html
```

---

### **Step 3: Create Server Block Configurations**

#### **Main Bookstore (theepicbooks.com)**
```bash
sudo nano /etc/nginx/sites-available/theepicbooks.com
```
```nginx
server {
    listen 80;
    server_name theepicbooks.com www.theepicbooks.com;
    root /var/www/theepicbooks.com/html;
    index index.html;
}
```

---

#### **Backend API (api.theepicbooks.com)**
```bash
sudo nano /etc/nginx/sites-available/api.theepicbooks.com
```
```nginx
server {
    listen 80;
    server_name api.theepicbooks.com;
    location / {
        proxy_pass http://127.0.0.1:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

---

### **Step 4: Enable Server Blocks and Restart Nginx**
```bash
sudo ln -s /etc/nginx/sites-available/theepicbooks.com /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/api.theepicbooks.com /etc/nginx/sites-enabled/
sudo systemctl reload nginx
```

✅ **Visit The EpicBook! in your browser:**
```
http://theepicbooks.com
http://api.theepicbooks.com
```

Now, let’s set up **Nginx as a load balancer** for scalability.

---

## **6. Load Balancing The EpicBook! API**

If The EpicBook! API runs on multiple servers, we can distribute traffic evenly.

```bash
sudo nano /etc/nginx/sites-available/load-balancer.conf
```
```nginx
upstream backend_servers {
    server 192.168.1.101;
    server 192.168.1.102;
}

server {
    listen 80;
    server_name api.theepicbooks.com;

    location / {
        proxy_pass http://backend_servers;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

---

## **7. Securing Nginx with HTTPS (Let’s Encrypt)**

### **Enable SSL with Certbot**
```bash
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d theepicbooks.com -d api.theepicbooks.com
```

✅ **Verify SSL Renewal**
```bash
sudo certbot renew --dry-run
```

---

## **Key Takeaways**

- Nginx is **lightweight, scalable, and optimized for high traffic**.
- Server blocks allow **hosting multiple websites**.
- Nginx can function as a **reverse proxy and load balancer**.
- SSL encryption ensures **secure communication**.

---

## **What’s Next?**

Now that you understand **Nginx**, the next step is:

- **Optimizing Nginx for performance and security.**  
- **Handling caching and implementing rate limiting.**  
- **Monitoring and troubleshooting Nginx logs.**

Let’s move forward and **apply these concepts in real-world deployments!**
