## Prerequisites
- A registered domain name.
- A DNS record with `your_domain.com` pointing to your proxy server’s public IP address.


## Convenient commands
Run the production version:
```
sudo docker compose -f docker-compose.prod.yml up --build -d
```
Migrate database:
```
sudo docker compose -f docker-compose.prod.yml exec django python manage.py migrate --noinput
```
Collect static files:
```
sudo docker compose -f docker-compose.prod.yml exec django python manage.py collectstatic
```

## Production security
To generate a secret key:
```
python3 -c "import secrets; print(secrets.token_urlsafe())"
```

## Get certificates from Let's Encrypt with certbot
Before acquiring the certificates:
- Comment out everything having to do with https in the Nginx config file.
- Comment out the Redirect to HTTPS location in the http section.

Do a dry run to test:
```
sudo docker compose -f docker-compose.prod.yml run --rm  certbot certonly --webroot --webroot-path /var/www/certbot/ --dry-run -d spaceengineering.io
```
And if it works, without the `--dry-run`:
sudo docker compose -f docker-compose.prod.yml run --rm  certbot certonly --webroot --webroot-path /var/www/certbot/ -d spaceengineering.io
```

After the successful acquiring of certificates:
- Uncomment everything having to do with https in the Nginx config file.
- Comment out the location which proxies to Django in the http section.
- Comment out the location with static files in the http section.

## Useful info for setup
- [Tutorial: Get started with Amazon EC2 Linux instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html) by AWS.
- [How To Install and Use Docker on Ubuntu 22.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-22-04) by Digital Ocean.
- [How To Scale and Secure a Django Application with Docker, Nginx, and Let's Encrypt](https://www.digitalocean.com/community/tutorials/how-to-scale-and-secure-a-django-application-with-docker-nginx-and-let-s-encrypt) by Digital Ocean.
- [Routing traffic to an Amazon EC2 instance](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-to-ec2-instance.html) by AWS.
- If using AWS and Route53, remember to [glue the nameservers records](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/domain-name-servers-glue-records.html) from the Hosted Zone to the registered domain.
- [Routing traffic to an Amazon EC2 instance](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-to-ec2-instance.html) by AWS.
- [HTTPS using Nginx and Let's encrypt in Docker](https://mindsers.blog/en/post/https-using-nginx-certbot-docker/)