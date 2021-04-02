docker images for personal use

---  

- [nginx](nginx/Dockerfile)  
  `blood72/nginx`  
  just added nginx-mod-http-headers-more  

- [supervisor for php](php-supervisor/Dockerfile)  
  `blood72/php:supervisor` or `blood72/php:7.4-supervisor`  
  an image for creating container using supervisor based on php \(default 7.4\)  
  USAGE: mount files \(like [supervisord.d](php-supervisor/supervisord.d) examples\) to `/etc/supervisord.d`  
