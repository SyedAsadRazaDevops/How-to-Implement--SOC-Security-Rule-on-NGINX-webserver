If you were securing Nginx with Mod Security, then you would like to have OWASP core rule set (CRS) activated to protect from following threats.

### Disable unwanted HTTP methods
Most of the time, you need just GET, HEAD & POST HTTP request in your web application. Allowing TRACE or DELETE is risky as it can allow Cross-Site Tracking attack and potentially allow a hacker to steal the cookie information.
	• Modify nginx.conf and add following under server block
````
if ($request_method !~ ^(GET|HEAD|POST)$ ) 
{
return 405; 
}
````

### Mask Nginx version details from the HTTP Response Header.
Add the following in nginx.conf under server section
```
server_tokens off;
````
	• Restart Nginx webserver

### Clickjacking Attack
You can inject X-FRAME-OPTIONS in HTTP Header to prevent a clickjacking attack.

This is achieved by adding below in nginx.conf file
````
add_header X-Frame-Options "SAMEORIGIN";
````
### X-XSS Protection
Inject HTTP Header with X-XSS protection to mitigate Cross-Site scripting attack.
	• Modify nginx.conf file to add the following
````
add_header X-XSS-Protection "1; mode=block";
````
### Cloudflare
If you are using Cloudflare, then you can enable HSTS in just a few clicks.
````
Go to the “Crypto” tab and click “Enable HSTS.”
````
#### Nginx
To configure HSTS in Nginx, add the next entry in nginx.conf under server (SSL) directive
````
add_header Strict-Transport-Security 'max-age=31536000; includeSubDomains; preload';
````
As usual, you will need to restart Nginx to verify

### X-Frame-Options

Use the X-Frame-Options header to prevent Clickjacking vulnerability on your website. By implementing this header, you instruct the browser not to embed your web page in frame/iframe. This has some limitations in browser support, so you got to check before implementing it.

Add the following in nginx.conf under server directive/block.
````
add_header X-Frame-Options “DENY”;
````
Restart to verify the results

### X-Content-Type-Options

Prevent MIME types of security risk by adding this header to your web page’s HTTP response. Having this header instructs browser to consider file types as defined and disallow content sniffing. There is only one parameter you got to add “nosniff”.

Add the following line in nginx.conf file under server block.
````
add_header X-Content-Type-Options nosniff;
````
As usual, you got to restart the Nginx to check the results.

### Content Security Policy

Prevent XSS, clickjacking, code injection attacks by implementing the Content Security Policy (CSP) header in your web page HTTP response. CSP instruct browser to load allowed content to load on the website.
All browsers don’t support CSP, so you got to verify before implementing it.

Add the following in the server block in nginx.conf file
````
add_header Content-Security-Policy "default-src 'self';";
````
### X-Permitted-Cross-Domain-Policies

Using Adobe products like PDF, Flash, etc.?
You can implement this header to instruct the browser on how to handle the requests over a cross-domain. By implementing this header, you restrict loading your site’s assets from other domains to avoid resource abuse.

And, let’s say you need to implement master-only then add the following in nginx.conf under server block.
````
add_header X-Permitted-Cross-Domain-Policies master-only;
````

### Permissions-Policy

Earlier known as Feature-Policy, it is renamed as Permissions-Policy with enhanced features. You can check out this to understand the big changes between Feature-Policy to Permissions-Policy.
With Permissions Policy, you can control browser features such as geolocation, fullscreen, speaker, USB, autoplay, speaker, microphone, payment, battery status, etc. to enable or disable within a web application. By implementing this policy, you let your server instruct a client (browser) to obey the web application functionality.

et’s take another example – disable vibrate feature.
````
add_header Permissions-Policy "vibrate 'none';";
````
Or, disable geolocation, camera, and speaker.
````
add_header Permissions-Policy "geolocation 'none'; camera 'none'; speak
````

### Clear Site Data

As you may guess by the name, implementing a Clear-Site-Data header is a great way to tell a client to clear browsing data such as cache, storage, cookies, or everything. This gives you more control over how you want to store the website’s data in the browser.

Let’s set Nginx to clear cookies.
````
add_header Clear-Site-Data "cookies";
````

** LINK **=
> https://geekflare.com/http-header-implementation/

> https://geekflare.com/nginx-webserver-security-hardening-guide/

![image](https://user-images.githubusercontent.com/71556060/199497858-12ae079d-bb4b-434e-b52d-0b9e8fa2fecf.png)
