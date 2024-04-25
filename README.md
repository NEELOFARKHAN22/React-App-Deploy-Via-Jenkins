# 1. Implement Auto-renew SSL:
```sh
I used this command: `sudo certbot renew --dry-run`  & used crone job for it  `* * */15 * * root certbot -q renew --apache >/dev/null 2>&1`
Manual way:   `sudo certbot certonly --force-renew -d freelyy.hopto.org -d freelyy.hopto.org`
```
# 2. Diff b/w certonly and renew in certbot:
 ```sh
The certonly generates the certificate per run per certificate. but renew can renew many certificates in one run. The other difference is certonly provides a user interaction facility via commands.
```
# 3. Try to implement a proxy pass:
# 4. Study On Directory Block:
```sh
The <virtualHost> contains directives referring to one host.

<VirtualHost *80>
`ServerAdmin doamin.com`
`DocumentRoot /usr/www/site.virtual/` 
`ServerName www.butterthlies.com` 
</VirtualHost>
```

### File and Location Options:

* If you need further control over specific files within a directory on your server, use the <Files> directive. This directive controls the behavior of the web server with regard to a single file. The <Files> directives will apply to any file with the specified name.
  
### Override Options with htaccess:

* In addition to the configuration methods discussed above, by default Apache will read configuration options for a directory from a file located in that directory. This file is typically called .htaccess
  
### Location_Options
* The directive limits the application of the directives within the block to those URLs specified, which can include wildcards and regular expressions preceded by "~". In line with regular expression processing in Apache v1.3, "*" and "?" no longer match to "/". is followed by a regular expression without the "~". Most things that are allowed in a block are allowed in , but although AllowOverride will not cause an error in a block, it makes no sense there. docstore.mik.ua/orelly/linux/apache/ch03_03.htm

# 5. DNS Record
```sh
Types-https://www.noip.com/support/knowledgebase/dns-host-types
```
# 6. a2enmod/a2dismod

* a2ensite  is a script that enables the specified site (which contains a <VirtualHost>block) within the apache2  configuration. it does this by creating symlinks within /etc/apache2/sites-enabled.
* a2dissite disables a site by removing those symlinks.
* a2ensite  and  a2dissite  exit with status 0 if all sites are processed successfully, 1 if errors occur, and 2 if an invalid option was used.
* a2dissite 000-default- Disables the default site.

# 7.a2enmod/a2dismod:a2enmod
* is a script that enables the specified module within the apache2 configuration. It does this by creating symlinks within  /etc/apache2/mods-enabled.   Likewise,  a2dismod disables a module by removing those symlinks.
* Eg.-a2enmod imagemap a2dismod mime_magic- Enables the mod_imagemap module, and disables the mod_mime_magic module.
  
  commit








  



