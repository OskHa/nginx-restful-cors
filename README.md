# RestFul CORS headers

## This is a working example of howto define CORS headers with allow-origin and allow-credentials.

```
# define the white list for http origin (in this exmplae only subdomains of base domain)
map $http_origin $cors_header {
    default "";
    "~^https?://[^/]+\.mycompany\.com(:[0-9]+)?$" "$http_origin";
}

server {
      listen 80;
      server_name api.mycompany.com;
      

      # CORS
      add_header Access-Control-Allow-Origin $cors_header;
      add_header 'Access-Control-Allow-Methods' 'POST, GET, PUT, DELETE, OPTIONS';
      add_header 'Access-Control-Allow-Credentials' 'true';
      add_header 'Access-Control-Allow-Headers' 'DNT,X-Mx-ReqToken,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type';
      

      # Default routing - try files then redirect to PHP
      location / {
           try_files $uri /app.php$is_args$args;

           # CORS
           add_header Access-Control-Allow-Origin $cors_header;
           add_header 'Access-Control-Allow-Methods' 'POST, GET, PUT, DELETE, OPTIONS';
           add_header 'Access-Control-Allow-Credentials' 'true';
           add_header 'Access-Control-Allow-Headers' 'DNT,X-Mx-ReqToken,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type';
      }
      
}
```
