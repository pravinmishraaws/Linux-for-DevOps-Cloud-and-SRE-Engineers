# Assignment 3: Deploy a React App on Ubuntu VM Using Nginx

## Objective
You’ve already deployed a static HTML site on **Amazon Linux**.  
Now, it’s time to deploy:

- A **[real React app](https://github.com/pravinmishraaws/my-react-app)** using **Ubuntu**
- A **<a href="https://github.com/pravinmishraaws/my-react-app" target="_blank">real React app</a>** using **Ubuntu**
- And serve it with **Nginx** from your VM's public IP

---

## What You'll Need
- A running **Ubuntu VM** (cloud or local)  
- Basic knowledge of Linux commands  
- Internet access from the VM  

---

## Step 1: Install Required Software

### Install Node.js & npm
```bash
sudo apt update
sudo apt install -y nodejs npm
node -v && npm -v
````

### Install and Start Nginx

```bash
sudo apt install -y nginx
sudo systemctl start nginx
sudo systemctl enable nginx
```

---

## Step 2: Clone the React App Repository

```bash
git clone https://github.com/pravinmishraaws/my-react-app.git
cd my-react-app
```

---

## Step 3: Modify the Application

Navigate to your React app’s source folder:

```bash
cd my-react-app/src
```

Open the `App.js` file:

```bash
nano App.js
# (or use vi/vim if you prefer)
```

Modify the content by updating **your name** and **date**:

```jsx
<h2>Deployed by: <strong>Your Full Name</strong></h2>
<p>Date: <strong>DD/MM/YYYY</strong></p>
```

---

## Step 4: Build the Application for Production

### Install Dependencies & Build App

```bash
npm install
npm run build
```

### Serve React App via Nginx

```bash
sudo rm -rf /var/www/html/*
sudo cp -r build/* /var/www/html/
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html
```

---

## Step 5: Configure Nginx

Replace the default config:

```bash
echo 'server {
  listen 80;
  server_name _;
  root /var/www/html;
  index index.html;

  location / {
    try_files $uri /index.html;
  }

  error_page 404 /index.html;
}' | sudo tee /etc/nginx/sites-available/default > /dev/null
```

Restart Nginx:

```bash
sudo systemctl restart nginx
```

---

## Step 6: Test the App

Find your public IP:

```bash
curl ifconfig.me
```

Open the app in your browser:
👉 [http://your-public-ip](http://<your-public-ip>)

---

## How to Submit Your Assignment

### For Live Cohort Students

Please follow the submission guidelines as explained during the session.

DevOps Micro Internship [Assignment Master Sheets](https://docs.google.com/spreadsheets/d/1HnlenHEjytvLJMy84bBF-5B1RABaY_BjbfwCj-qnvHM/edit)


