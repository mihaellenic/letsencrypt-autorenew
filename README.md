# letsencrypt-autorenew
LetsEncrypt SSL cert renewal using dedicated HTTP server and autorenewal script

# Quick Guide
1. Run dedicated HTTP server
```
docker run --name letsencrypt-dedicated-http -v <path-to-challenge-folder>:/opt/letsencrypt/challenge -d mlenic/letsencrypt-dedicated-http
```

2. Fetch SSL certificates
```
docker run -it --rm -v <path-to-letsencrypt-folder>:/etc/letsencrypt -v <path-to-challenge-folder>:/opt/challenge certbot/certbot certonly --webroot -w /opt/challenge -d <domain-name> --email <your-email> --agree-tos 
```

3. Replace `<path-to-letsencrypt>` in `renew.sh` with LetsEncrypt path used in step #2
```
docker run -it --rm -v <path-to-letsencrypt>:/etc/letsencrypt certbot/certbot renew
```

4. Setup automatic SSL certificates renewal (once a week)
```
$ crontab -e
* 10 * * 2 <path-to-renew.sh>
```

## Notes
- both `challenge` and `letsencrypt` folders should be empty before starting the steps
- to fetch certificates for multiple domains just add another `-d` parameter in step #2
```
docker run -it --rm -v <path-to-letsencrypt-folder>:/etc/letsencrypt -v <path-to-challenge-folder>:/opt/challenge certbot/certbot certonly --webroot -w /opt/challenge -d <domain-name> -d <second-domain-name> -d <third-domain-name> --email <your-email> --agree-tos 
```
- you can create your docker image with custom nginx configuration. Just update `nginx.conf` and build docker image from `Dockerfile`

## Official CertBot documentation
https://certbot.eff.org/docs/