# Apache Name-Based Virtual Host on Debian

**Date**: 2025-09-08  
**Environment**: Debian 12, running inside a TrueNAS SCALE virtual machine. 
**Host machine**: Intel i5, 8GB RAM, 50GB disk. 


---

## a) Install and Test Apache

Install Apache:

```bash
sudo apt update
sudo apt install apache2 -y
```

Checked service status:
```bash
sudo systemctl status apache2
```
<img width="800" height="268" alt="image" src="https://github.com/user-attachments/assets/016a511b-7a1c-4b63-8b96-82a54845861c" />


Tested that the server is working:
   I opened my preferred browser and navigated to my web page at http://<IP_ADDRESS>
<img width="802" height="426" alt="default Page" src="https://github.com/user-attachments/assets/5b87cef6-3e83-4398-81c4-8b7db49b0afc" />





## b) Analyze Apache Access Logs

View access logs in real time:

sudo tail -f /var/log/apache2/access.log

I went to the site again.
<img width="1708" height="148" alt="image" src="https://github.com/user-attachments/assets/f8f210b2-7235-47b3-80aa-f59435778e08" />

| Part                              | Description                                                             |
| --------------------------------- | ----------------------------------------------------------------------- |
| `10.10.40.46`                     | Client's IP address (private IP on local network)                       |
| `[08/Sep/2025:04:18:37 -0400]`    | Timestamped in day/month/year\:hour\:minute\:second timezone format     |
| `"GET / HTTP/1.1"`                | The HTTP request method, requested path, and protocol version           |
| `200`                             | HTTP status code (200 = OK)                                             |
| `3383`                            | Size of the response in bytes                                           |
| `"Mozilla/5.0 ... Firefox/142.0"` | User-Agent string — identifies the browser                              |


**Conclusion**: Everyting is working  as it should





