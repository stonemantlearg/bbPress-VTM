# bbPress-VTM
Posible foro con ficha de VTM


#

sudo apt update<br>
sudo pat upgrade<br>
sudo apt install php apache2 php-mysql mariadb-server postfix libsas12-modules -y<br>
cd /var/www/html/<br>
rm ./*<br>
sudo wget http://wordpress.org/latest.tar.gz<br>
sudo tar xzf latest.tar.gz<br>
sudo mv wordpress/* .<br>

sudo mysql -uroot -p $PASSWORDVAR<br>
create database wordpress;<br>
GRANT ALL PRIVILEGES ON wordpress.* TO 'root'@'localhost' IDENTIFIED BY '$PASSWORDVAR'<br>
FLUSH PRIVILEGES<br>

sudo service apache2 restart<br>
cd /etc/postfix/sasl<br>
sudo nano -B sasl_passwd ## Inserta password y usuario correspondientes al SMTP en este formato: "[smtp.gmail.com]:587 <user>@gmail.com:<password>"<br>
sudo chmod u=rw,go= /etc/postfix/sasl/sasl_passwd<br>
sudo postmap /etc/postfix/sasl/sasl_passwd<br>
sudo cp /etc/postfix/main.cf !#$dist<br>
sudo nano /etc/postfix/main.cf<br>

Aquí se agrega al archivo:<br>
<code>relayhost = [smtp.gmail.com]:587</code> <br>
<code>## #Enable authentication using SASL </code><br>
<code>## smtp_sasl_auth_enable=yes </code><br>
<code>## #Use transport layer security (TLS) encryption </code><br>
<code>## smtp_tls_security_level=encrypt </code><br>
<code>## #Do not allow anonymous authentication</code> <br>
<code>## smtp_sasl_security_options=noanonymous</code> <br>
<code>## #Specify where to find the login information </code><br>
<code>## smtp_sasl_password_maps=hash:/etc/postfix/sasl/sasl_passwd </code><br>
<code>## #Where to find the certificate authority(CA) certificate </code><br>
<code>## smtp_tls_CAfile=/etc/ssl/certs/ca_certificates.cert </code>

sudo service postfix restart<br>
sudo postfix reload<br>


Troubleshoot en caso de no funcionar:<br><br>

ping -c 5 smtp.gmail.com -> Si no responde es un problema de conexión<br>
sendmail <mail>@gmail.com -> Permite escribir un mail, se declara primero Subject: X, enters pasan a la siguiente linea y se sale tras poner un punto (.)

