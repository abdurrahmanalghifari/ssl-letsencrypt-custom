# Nginx LetsEncrypt Custom

[Let's Encrypt](https://letsencrypt.org) is a free, automated, and open certificate authority brought to you by the non-profit Internet Security Research Group (ISRG).

``` sh
yum -y install epel-release certbot
mkdir -p /opt/stagging/nginx/conf/.well-known/acme-challenge
``` 



### conf taru di setiap vhost(subdomain)
nanti akan kita daftarkan semua subdomain pada domain tersebut

    # Blok sebelum tutup paling bawah
    #---ACME WELLKNOWN---#
    location /.well-known {
          alias /opt/stagging/nginx/conf/.well-known;
    }

### Generate SSL 
lalu jalankan perintah ini untuk generate semua subdomain. silahkan listing kebutuhannya.
```sh
certbot certonly --rsa-key-size 4096 --webroot --agree-tos --no-eff-email --email age@domain.com -w /opt/stagging/nginx/conf -d domain.com -d www.domain.com -d cms.domain.com -d apis.domain.com
```

### Copy SSL .key & .Pem
``` sh 
cp -a /etc/letsencrypt/archive/domain.com /opt/stagging/nginx/conf/ssl/
```


### Enable Konfigurasi

``` sh
#SSL LETSENCRYPT
ssl_certificate /opt/stagging/nginx/conf/ssl/domain.com/fullchain1.pem;
ssl_certificate_key /opt/stagging/nginx/conf/ssl/domain.com/privkey1.pem;
```

### Test Nginx & Reload
# Finish
#######################################


### Enabling Cronjob & Autorenew
```sh 
crontab -e
#“At 11:00 on Monday.”
0 11 * * 1 certbot renew
```

Salam,
**abdurrahman al ghifari**
