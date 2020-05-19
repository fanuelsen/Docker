# Træfik 2
## Files
| Name | Usage |
| ---- | ---------- |
| docker-compose.yml | Compose file for Træfik, http redirection to https with letsencrypt enabled and dashboard. |
| /traefik/static.yml | Træfik static configuration file |
| /traefik/dynamic.yml | Træfik static configuration file |
| /traefik/access.log | Træfik access logs |
| /traefik/traefik.log | Træfik debug logs |
| /letsencrypt/acme.json | Letsencrypt Certificates (chmod 600 acme.json)|
## docker-compose.yml

### Volumes
| Lines | Usage | 
| ---- |----| 
| ```/etc/localtime:/etc/localtime:ro``` | Sets container time same as host |
| ```./letsencrypt:/letsencrypt``` | Shared folder with certs in json format (chmod 600 on acme.json) |
| ```./traefik/traefik.log:/traefik.log``` | Traefik debug log |
| ```./traefik/access.log:/access.log``` | Access log |
| ```./traefik/static.yaml:/static.yaml``` | Initial static config |
| ```./traefik/dynamic.yaml:/etc/traefik/dynamic.yaml``` | Dynamic config (min tls version and sniStrict) |

### Rules and middlewares asociation with entrypoints
| Lines | Usage | 
| ---- |----| 
| ```traefik.http.routers.http-catchall.rule=hostregexp(`{host:.+}`)``` | Catchs all request for http |
| ```traefik.http.routers.http-catchall.entrypoints=web``` | Http middleware for web (http) entrypoint (see static.yaml file) |
| ```traefik.http.routers.http-catchall.middlewares=redirect-to-https,security-headers``` | Add redirect and security middlewares |
| ```traefik.http.routers.https-catchall.rule=hostregexp(`{host:.+}`)``` | Catchs all request for https |
| ```traefik.http.routers.https-catchall.entrypoints=websecure``` | Http middleware for websecure (https) entrypoint (see static.yaml file) |
| ```traefik.http.routers.https-catchall.middlewares=security-headers``` | Add security middleware |

### Secure Dashboard (http basic auth) just with localhost
| Lines | Usage | 
| ---- |----| 
| ```traefik.http.routers.traefik.rule=Host(`traefik.localhost`)``` | Requestpoint to localhost (if accessing from another computer set traefik in localhost file) |
| ```traefik.http.routers.traefik.service=api@internal``` | Specifiying the dashboard |
| ```traefik.http.routers.traefik.middlewares=admin``` | Use the admin middleware |
| ```traefik.http.routers.traefik.entrypoints=web``` | access via http 'http://traefik/dashbaord/' remember the trailing slash |

### middlewares asociation with dashboard
| Lines | Usage | 
| ---- |----| 
| ```traefik.http.middlewares.admin.basicauth.users=admin:$$apr1$$SAMaaHjX$$ZCo51QWKtqCOzrfaSZxwc1``` | Basic auth for dashboard - user admin password admin. Use [ echo $(htpasswd -nb user password) | sed -e s/\\$/\\$\\$/g ] to generate new password

## Data folder and config
```
mkdir -p ./traefik/letsencrypt
mkdir -p ./traefik/traefik/
touch ./lestencrypt/acme.json
touch ./traefik/traefik/access.log
touch ./traefik/traefik/traefik.log
chmod 600 ./letsencrypt/acme.json
docker network create proxy
docker-compose up -d
```

