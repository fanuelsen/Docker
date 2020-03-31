# Træfik 2
##Files
| Name | Usage |
| ---- | ---------- |
| docker-compose.yml | Compose file for Træfik, http redirection to https with letsencrypt enabled and dashboard exposed with basic auth. |
| /data/traefik.yml | Træfik configuration file|

##docker-compose.yml
| Lines | Usage | 
| ---- |----| 
| ``` "traefik.http.routers.traefik.rule=Host(`traefik.example.org`)" ``` | Change the host rule to your subdomain. |
| ```"traefik.http.middlewares.traefik-auth.basicauth.users=user:$$apr1$$q8eZFHjF$$Fvmkk//V6Btlaf2i/ju5n/"``` | Ccredentials in htpasswd ```echo $(htpasswd -nbB <USER> "<PASS>") | sed 's/\$/\$\$/g')``` currently user pass |
|```"traefik.http.routers.traefik-secure.rule=Host(`traefik.example.org`)"```| Change the host rule to your subdomain |

##Data folder and config

```
mkdir -p ./traefik/data
touch ./data/acme.json
chmod 600 ./data/acme.json
docker network create proxy
docker-compose up -d
```

