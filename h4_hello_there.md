# Hello_there

## a) Renting a Virtual Server
- I chose to use **UpCloud** because our teacher mentioned it's trustworthy, and I personally like the fact that it is a **Finland-based** hosting provider.
- I created an account and selected a system that would meet my needs.

Specifications:

- **CPU:** 1 core  
- **Memory:** 1 GB  
- **Storage:** 20 GB  
- **Location:** Finland (Helsinki)  
- **Operating System:** Debian 11

- **Added my public ssh key the platform makes changes to the system!!!!!!!!!!!!!!!!!!!!**

---

## b) Initial Server Setup

I connected to my server using SSH and began setting it up for secure operation.  I made user called schooldeb

### 1. Create a new user and give sudo access

```bash
sudo adduser schooldeb
sudo usermod -aG sudo schooldeb
```

### 2. Copy SSH key to the new user
```bash
sudo rsync --archive --chown=schooldeb:schooldeb ~/.ssh /home/schooldeb
```
- How to Use Rsync to Make a Remote Linux Backup, 2023)

### 3. Lock the root account
```bash
sudo usermod --lock root
sudo mv -nv /root/.ssh /root/DISABLED-ssh/
sudo nano /etc/ssh/sshd_config
```
- Find and chage line (PermitRootLogin yes) to (PermitRootLogin no)
- **Before
<img width="225" height="123" alt="image" src="https://github.com/user-attachments/assets/277f982a-ca1a-4735-a161-09970c49ce9b" />
- **After
<img width="181" height="123" alt="image" src="https://github.com/user-attachments/assets/d0220644-3c23-4bce-ac19-f07e93d71546" />

### 4. Install and configure the firewall (UFW)
```bash
sudo apt update
sudo apt install ufw -y
sudo ufw allow OpenSSH
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp # if you are planning to use HTTPS (http over TLS/SSL)
sudo ufw enable
```
### 5. Update the system and reboot
```bash
sudo apt update
sudo apt upgrade -y
sudo apt dist-upgrade -y
sudo systemctl reboot
```
### 6. Testing everything

**Check firewall status**:
```bash
sudo ufw status verbose
```
<img width="491" height="137" alt="image" src="https://github.com/user-attachments/assets/59f8e9ad-83f6-4d26-92c3-7a11b904f468" />
**Check if root is locked**:
```bash
sudo passwd -S root
```
** L = Locked
<img width="535" height="43" alt="image" src="https://github.com/user-attachments/assets/552fd551-897b-4caa-8069-9688864c4e45" />

**Check OS version**:
```bash
lsb_release -a
```
<img width="413" height="127" alt="image" src="https://github.com/user-attachments/assets/1c21acbb-3e3f-4da0-8657-efc5c7bc11e2" />

## c) Installing and Testing the Web Server
- I installed the Apache web server and replaced the default test page from my old h3 task.
- Verified that it is visible publicly on both desktop and mobile.
  
### 1. Install Apache
```bash
sudo apt install apache2 -y
```
### 2. Replace the default page
```bash
sudo nano /var/www/html/index.html
```
- Enter the HTML content of your choice.
- I used the HTML from the last H3 submission.

3. Restart Apache
```bash
sudo systemctl restart apache2
```
4. Check the site from browser
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!




'





I opened the browser of choice and navigated to:

http://[your_server_ip]

The page displayed the message: Hello from my VPS. I also tested it on my mobile phone using mobile data to ensure it worked from the outside network.

📷 Screenshot – Web page in browser:
___ss here
d) Name Based Virtual Host (Optional)

I also configured a Name Based Virtual Host, which allows me to host multiple websites on the same server and manage them as a normal user.
1. Create a directory and custom test page

sudo mkdir -p /var/www/mydomain.fi/html
sudo chown -R $USER:$USER /var/www/mydomain.fi/html
echo "<h1>This is mydomain.fi</h1>" > /var/www/mydomain.fi/html/index.html

2. Create a new Virtual Host config file

sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/mydomain.fi.conf
sudo nano /etc/apache2/sites-available/mydomain.fi.conf

Then I changed the content of the config file to:

<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    ServerName mydomain.fi
    DocumentRoot /var/www/mydomain.fi/html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

3. Enable the new site

sudo a2ensite mydomain.fi.conf
sudo systemctl reload apache2

4. Disable the default site

sudo a2dissite 000-default.conf
sudo systemctl reload apache2

After this, only my custom site at mydomain.fi was visible regardless of which domain was accessed.

📷 Screenshot – mydomain.fi in browser:
___ss here





links:
https://www.geeksforgeeks.org/linux-unix/how-to-use-rsync-to-make-a-remote-linux-backup/

How to Use Rsync to Make a Remote Linux Backup
Last Updated : 13 Dec, 2023 
