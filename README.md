# bbPress-VTM
Posible foro con ficha de VTM


#

sudo apt update
sudo pat upgrade
sudo apt install php apache2 php-mysql mariadb-server postfix libsas12-modules -y
cd /var/www/html/
rm ./*
sudo wget http://wordpress.org/latest.tar.gz
sudo tar xzf latest.tar.gz
sudo mv wordpress/* .

sudo mysql -uroot -p $PASSWORDVAR
create database wordpress;
GRANT ALL PRIVILEGES ON wordpress.* TO 'root'@'localhost' IDENTIFIED BY '$PASSWORDVAR'
FLUSH PRIVILEGES

sudo service apache2 restart
cd /etc/postfix/sasl
sudo nano -B sasl_passwd ## Inserta password y usuario correspondientes al SMTP en este formato: "[smtp.gmail.com]:587 <user>@gmail.com:<password>"
sudo chmod u=rw,go= /etc/postfix/sasl/sasl_passwd
sudo postmap /etc/postfix/sasl/sasl_passwd
sudo cp /etc/postfix/main.cf !#$dist
sudo nano /etc/postfix/main.cf

## Aquí se agrega al archivo:
## relayhost = [smtp.gmail.com]:587
## #Enable authentication using SASL
## smtp_sasl_auth_enable=yes
## #Use transport layer security (TLS) encryption
## smtp_tls_security_level=encrypt
## #Do not allow anonymous authentication
## smtp_sasl_security_options=noanonymous
## #Specify where to find the login information
## smtp_sasl_password_maps=hash:/etc/postfix/sasl/sasl_passwd
## #Where to find the certificate authority(CA) certificate
## smtp_tls_CAfile=/etc/ssl/certs/ca_certificates.cert

sudo service postfix restart
sudo postfix reload


Troubleshoot en caso de no funcionar:

ping -c 5 smtp.gmail.com -> Si no responde es un problema de conexión
sendmail <mail>@gmail.com -> Permite escribir un mail, se declara primero Subject: X, enters pasan a la siguiente linea y se sale tras poner un punto (.)

