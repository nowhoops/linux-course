# Apache Name-Based Host on Debian

**Date**: 2025-09-08  
**Environment**: Debian 12, running inside a TrueNAS SCALE virtual machine. 
**Host machine**: Intel i5, 8GB RAM, 50GB disk. 


## x)
### **1.**
**Add New Name Based Virtual Host** 
- Allows hosting multiple websites
- Allows hosting with multiple domain names
- First listed VirtualHost becomes the default if no match is found.
  - Websites ar created in separate direcotires under user's home foler
  - Easy testing with curl
  (tero, karvinen)
###  **2.**
**Name-Based Virtual Hosts on Apache**
- Apache first matches a request by IP and port, then uses ServerName and ServerAlias to select the correct virtual host.
- Always specify a ServerName in each virtual hostto avoid odd behavior, from inherited or default values.
- Easy to have multible instances
## a) Install and Test Apache

---
**Connected via ssh**
```bash
ssh shooldeb@<IP_ADDRESS>
<Enter your password:>
```

Install Apache:

```bash
sudo apt update
sudo apt install apache2 -y
```

Checked service status:
```bash
sudo systemctl status apache2
```
**Screenshot of the terminal**
<img width="800" height="268" alt="image" src="https://github.com/user-attachments/assets/016a511b-7a1c-4b63-8b96-82a54845861c" />


Tested that the server is working:
   I opened my preferred browser and navigated to my web page at http://<IP_ADDRESS>
<img width="802" height="426" alt="default Page" src="https://github.com/user-attachments/assets/5b87cef6-3e83-4398-81c4-8b7db49b0afc" />





## b) Analyze Apache Access Logs

Access logs:

sudo tail -f /var/log/apache2/access.log

I went to the site again.
<img width="1708" height="148" alt="image" src="https://github.com/user-attachments/assets/f8f210b2-7235-47b3-80aa-f59435778e08">

| Part                              | Description                                                             |
| --------------------------------- | ----------------------------------------------------------------------- |
| `10.10.40.46`                     | Client's IP address (private IP on local network)                       |
| `[08/Sep/2025:04:18:37 -0400]`    | Timestamped in day/month/year\:hour\:minute\:second timezone format     |
| `"GET / HTTP/1.1"`                | The HTTP request method, requested path, and protocol version           |
| `200`                             | HTTP status code (200 = OK)                                             |
| `3383`                            | Size of the response in bytes                                           |
| `"Mozilla/5.0 ... Firefox/142.0"` | User-Agent string — identifies the browser                              |


**Conclusion**: Everyting is working  as it should

## c) Replacerf Default Page with Name-Based Virtual Host
1. Created new directory for the site

mkdir -p ~/public_html/hattu.example.com

2. Created a HTML page
```bash
nano ~/public_html/hattu.example.com/index.html
```
Pasted the following:
```bash
                                     
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>HATS</title>
  <style>
    /* Reset */
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      background: linear-gradient(135deg, #ff7e5f, #feb47b);
      font-family: 'Poppins', sans-serif;
      overflow: hidden;
    }

    h1 {
      font-size: 6rem;
      color: white;
      text-transform: uppercase;
      letter-spacing: 0.5rem;
      cursor: pointer;
      transition: transform 0.3s ease, color 0.3s ease;
    }

    h1:hover {
      transform: scale(1.2) rotate(-5deg);
      color: #ffd700;
    }


    .circle {
      position: absolute;
      border-radius: 50%;
      background: rgba(255, 255, 255, 0.3);
      pointer-events: none;
      animation: float 5s infinite ease-in-out;
    }

    @keyframes float {
      0%, 100% { transform: translateY(0); }
      50% { transform: translateY(-20px); }
    }
  </style>
</head>
<body>
  <h1 id="hat-text">HATS</h1>

  <script>
   
    document.addEventListener('mousemove', (e) => {
      const circle = document.createElement('div');
      circle.className = 'circle';
      circle.style.width = circle.style.height = `${Math.random() * 50 + 10}px`;
      circle.style.left = `${e.clientX - 25}px`;
      circle.style.top = `${e.clientY - 25}px`;
      circle.style.background = `rgba(255, 255, 255, ${Math.random() * 0.5 + 0.2})`;
      document.body.appendChild(circle);

      setTimeout(() => {
        circle.remove();
      }, 1000);
    });


    const hatText = document.getElementById('hat-text');
    hatText.addEventListener('click', () => {
      hatText.style.color = `hsl(${Math.random() * 360}, 100%, 70%)`;
    });
  </script>
</body>
</html>
```
3. Set proper permissions
```bash
chmod ugo+x $HOME $HOME/public_html
```
4. Created virtual host config
```bash
sudo nano /etc/apache2/sites-available/hattu.conf
```
Add:
```bash
<VirtualHost *:80>
    ServerName hattu.example.com
    DocumentRoot /home/yourusername/public_html/hattu.example.com
    <Directory /home/yourusername/public_html/hattu.example.com>
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
    </Directory>
</VirtualHost>
```
Replace yourusername with your actual Linux username.

5. Edit hosts file to simulate DNS

sudo nano /etc/hosts

Add line:
- these to simulate dns.
```bash
127.0.0.1    webapp.home.hlf
127.0.0.1    www.webapp.home.hlf
127.0.0.1    webapp.home.hlf
127.0.0.1    www.webapp.home.hlf
```
- I use my home network's own dns server, because it is configured to prevent potential DNS spoofing.
**Example simulated dns**
<img width="567" height="93" alt="image" src="https://github.com/user-attachments/assets/d56b3aee-0ff1-43dc-91ef-c1acab4c65e4" />

6. Enable new site, disable default
```bash
sudo a2dissite 000-default.conf
sudo a2ensite hattu.conf
sudo systemctl restart apache2
```

Visited (http://webapp.home.hlf/)]

<img width="1065" height="937" alt="image" src="https://github.com/user-attachments/assets/d2f69a87-1405-4009-be03-6a45d29d016a" />

 e) Validated the HTML5 Page

Go to https://validator.w3.org/

     I checked the “Validate by direct input”

    Pasted HTML 
**Result:** 
<img width="1065" height="839" alt="image" src="https://github.com/user-attachments/assets/681faae5-3a35-4b6a-a10f-cbdce2df9b89" />

f) Use curl and Explain Headers
1. View response headers 
curl -I http://hattu.example.com

My output:
schooldeb@schooldeb:~$ curl -I http://webapp.home.hlf
HTTP/1.1 200 OK
Date: Mon, 08 Sep 2025 15:19:55 GMT
Server: Apache/2.4.65 (Debian)
Last-Modified: Mon, 08 Sep 2025 15:12:36 GMT
ETag: "77a-63e4b9e1e4596"
Accept-Ranges: bytes
Content-Length: 1914
Vary: Accept-Encoding
Content-Type: text/html

Header Explanation
| **Header**       | **Value**                       | **Explanation**                                                              |
| ---------------- | ------------------------------- | ---------------------------------------------------------------------------- |
| `HTTP/1.1`       | `200 OK`                        | The request was successful.                                                  |
| `Date`           | `Mon, 08 Sep 2025 15:19:55 GMT` | The time when the server sent the response.                                  |
| `Server`         | `Apache/2.4.65 (Debian)`        | Shows the web server software and version I use (Apache on Deban).           |
| `Last-Modified`  | `Mon, 08 Sep 2025 15:12:36 GMT` | Time when the file was last changed                                          |
| `Content-Length` | `1914`                          | Size of the response body in bytes                                           |
| `Content-Type`   | `text/html`                     | Indicates that the response is HTML, so the browser knows how to render it.  |




**extras**
Made too directorys
```bash
mkdir -p ~/public_html/foo
mkdir -p ~/public_html/bar
```
Added different index.html to each.
```bash
echo "<Used the same html as before>" | sudo tee /var/www/foo.home.hlf/public_html/index.html
echo "<Used the same html as before changed some colors(not nessessary)>" | sudo tee /var/www/foo.home.hlf/public_html/index.html

```
2. Create two config files:

**Edit conf file**
sudo nano /etc/apache2/sites-available/foo.conf

```bash
<VirtualHost *:80>
    ServerName foo.home.hlf
    DocumentRoot /home/yourusername/public_html/foo
    <Directory /home/yourusername/public_html/foo>
        Require all granted
    </Directory>
</VirtualHost>
```
```bash
**Edit conf file**
sudo nano /etc/apache2/sites-available/bar.conf
```
```bash
<VirtualHost *:80>
    ServerName bar.home.hlf
    DocumentRoot /home/yourusername/public_html/bar
    <Directory /home/yourusername/public_html/bar>
        Require all granted
    </Directory>
</VirtualHost>
```

3. Add to /etc/hosts:
**Examples I'm using my dns server**
```bash
127.0.0.1 foo.home.hlf   
127.0.0.1 bar.home.hlf
```

5. Enable both:
```bash
sudo systemctl reload apache2
sudo a2ensite foo.conf
sudo a2ensite bar.conf
```

Test both in your browser:

    **http://foo.home.hlf**
<img width="1279" height="1363" alt="image" src="https://github.com/user-attachments/assets/c80cc57f-7db2-4e1a-b962-ef74f54f0546" />


   **http://bar.home.hlf**
<img width="1279" height="1363" alt="image" src="https://github.com/user-attachments/assets/83a016ce-23c3-4d2d-a84e-95d832ea5f70" />


**m)** 
- **Done**

##**Sources:**

- Karvinen, Tero 2012: Linux course – http://terokarvinen.com/
- Debian Apache2 Wiki, 2021 - https://wiki.debian.org/Apache
- W3C HTML5 Validator, https://validator.w3.org/
- man curl
- https://edu.gcfglobal.org/en/basic-html/interactive-elements-in-html/1/







