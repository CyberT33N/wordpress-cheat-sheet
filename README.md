# Wordpress Cheat Sheet
Wordpress Cheat Sheet with the most needed stuff..


## .htaccess

```bash
# disable directory browsing
Options All -Indexes

<files ~ "^.*\.([Hh][Tt][Aa])">
order allow,deny
deny from all
satisfy all
</files>





# Don't show errors which contain full path diclosure (FPD)
# Use that line only if PHP is installed as a module and not per CGI
# try using a php.ini in that case.
# Change mod_php5.c to mod_php7.c if you are running PHP7
<IfModule mod_php5.c>
  php_flag display_errors Off
</IfModule>

# Don't list directories
<IfModule mod_autoindex.c>
  Options -Indexes
</IfModule>

# Protect XMLRPC (needed for Apps, Offline-Blogging-Tools, Pingback, etc.)
# If you use that, these tools will not work anymore
<Files xmlrpc.php>
  Order Deny,Allow
  Deny from all
</Files>

# If you don't use the Database Optimizing and Post-by-Email features, turn off the access too:
<FilesMatch "(repair|wp-mail)\.php">
    Order Deny,Allow
    Deny from all
</FilesMatch>

# Prevent browser and search engines to request .log (e.g. WP DEBUG LOG) and .txt (e.g. plugins readme) files. 
# Must be placed in /wp-content/.htaccess
<FilesMatch "\.(log|txt)$">
    Order Allow,Deny
    Deny from all
</FilesMatch>

# Hide WordPress, system & sensitive files
<FilesMatch "(^\.|wp-config(-sample)*\.php)">
    Order Deny,Allow
    Deny from all
</FilesMatch>

# Protect some other files
<FilesMatch "(liesmich.html|readme.html|(.*)\.ttf|(.*)\.bak)">
  Order Deny,Allow
  Deny from all
</FilesMatch>

# Block the include-only files.
# Do not use in Multisite without reading the note in Codex!
# See: https://codex.wordpress.org/Hardening_WordPress#WP-Includes
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteBase /
  RewriteRule ^wp-admin/includes/ - [F,L]
  RewriteRule !^wp-includes/ - [S=3]
  # If you run multisite, comment the next line (see note above)
  RewriteRule ^wp-includes/[^/]+\.php$ - [F,L]
  RewriteRule ^wp-includes/js/tinymce/langs/.+\.php - [F,L]
  RewriteRule ^wp-includes/theme-compat/ - [F,L]
</IfModule>

# Set some security related headers
# See: http://de.slideshare.net/walterebert/die-htaccessrichtignutzenwchh2014 (GERMAN)
<IfModule mod_headers.c>
  Header set X-Content-Type-Options nosniff 
  Header set X-XSS-Protection "1; mode=block" 
  # The line below is an advanced method for a more secure configuration, please see documentation before usage!
  # Introduction: https://scotthelme.co.uk/content-security-policy-an-introduction/
  # http://www.heise.de/security/artikel/XSS-Bremse-Content-Security-Policy-1888522.html (German)
  # Documentation: https://content-security-policy.com/
  # Analysis: https://securityheaders.io/
  # Header set Content-Security-Policy "default-src 'self'; img-src 'self' http: https: *.gravatar.com;"
</IfModule>

# Allow WordPress Embed
# https://gist.github.com/sergejmueller/3c4351ec29576fb441fe
<IfModule mod_setenvif.c>
    SetEnvIf Request_URI "/embed/$" IS_embed
    <IfModule mod_headers.c>
    	Header set X-Frame-Options SAMEORIGIN env=!REDIRECT_IS_embed
    </IfModule>
</IfModule>

#Force secure cookies (uncomment for HTTPS)
<IfModule mod_headers.c>
  #Header edit Set-Cookie ^(.*)$ $1;HttpOnly;Secure
</IfModule>

#Unset headers revealing versions strings
<IfModule mod_headers.c>
  Header unset X-Powered-By
  Header unset X-Pingback
  Header unset SERVER
</IfModule>

# Filter Request Methods
# See: https://perishablepress.com/disable-trace-and-track-for-better-security/
<IfModule mod_rewrite.c>
  RewriteEngine on
  RewriteCond %{REQUEST_METHOD} ^(TRACE|DELETE|TRACK) [NC]
  RewriteRule ^(.*)$ - [F,L]
</IfModule>
# BEGIN WordPress
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
RewriteRule ^index\.php$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /index.php [L]
</IfModule>

# END WordPress
```




<br />
<br />


 _____________________________________________________
 _____________________________________________________


<br />
<br />

# Permissions


## CHMOD
Set all directories permissions to 755:
```bash
find . -type d -exec chmod 755 {} \;
```
<br />
Set all files permissions to 644
```bash
find . -type f -exec chmod 644 {} \;
```
<br />
Next change the full project owner:
```bash
sudo chown t33n:t33n -R * /var/www/html/yourwebsitehere
```
<br />
Those files need www-data usergroup for public access
```bash
sudo chown t33n:www-data /var/www/html/yourwebsitehere/wp-config.php
sudo chown t33n:www-data -R * /var/www/html/yourwebsitehere/wp-content
```

<br />
wp-config give 400 permissions
**IMPORANT** This only works when the server is running.. when you restart apache then its not working again and you get error 500
<br />So you better give 440







<br />
<br />


 _____________________________________________________
 _____________________________________________________


<br />
<br />
