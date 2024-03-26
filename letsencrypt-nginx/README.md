# Setting Up Nginx SSL with Let's Encrypt Certbot on Docker

## Usage Guide

### Domain Setup

Ensure that your domain is registered and properly pointed to your server's IP address.

### Configuration Update

Modify the domain name in the Nginx configuration file located in the `nginx/conf` directory.

### Launching the Nginx Web Server

Execute the following command to start the web server:

```bash
docker compose -f init.docker-compose.yml up -d webserver
```

### Certbot Dry Run

Replace `yourdomain.com` with your actual domain name and perform a dry run:

```bash
docker compose run --rm certbot certonly --webroot --webroot-path /var/www/certbot/ --dry-run -d yourdomain.com
```

If successful, you'll see a message stating, "The dry run was successful." If it fails, verify your domain's DNS settings or review the Nginx configuration file for errors.

### Obtaining the SSL Certificate

After a successful dry run, replace `yourdomain.com` with your domain to obtain the SSL certificate:

```bash
docker compose run --rm certbot certonly --webroot --webroot-path /var/www/certbot/ -d yourdomain.com
```

### Restarting the Nginx Web Server

To apply the changes, restart the web server using one of the following commands:

```bash
docker compose restart webserver
```

or

```bash
docker compose exec webserver nginx -s reload
```

## Maintenance Tips

### Manual Certificate Renewal

Manually renew the certificate with this command:

```bash
docker compose run --rm certbot renew
```

### Automated Renewal with Crontab

Edit your crontab with `crontab -e` and add the following line to schedule automatic renewal:

```bash
0 5 1 */2 * /usr/local/bin/docker compose -f /path/to/your/docker-compose.yml run --rm certbot renew
```

This will execute the renewal command at 5 AM on the first day of every second month.

Note: Ensure to replace `<docker-compose.yml file>` with the actual path to your `docker-compose.yml` file.
