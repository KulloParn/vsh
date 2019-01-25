esiteks laadisin alla mysql serveri ja php5
apt-get install mysql-server php-mysql

laadisin alla php mooduleid
apt-get install php5-common libapache2-mod-php5 php5-cli

tegin apachele restardi
service apache2 restart

vahepeal laadisin alla sudo, kuna oled sellega ära harjunud
apt-get install sudo

laadisin alla mysql
sudo apt install mysql-server

laadisin alla phpmyadmini
sudo apt install mcrypt
sudo /etcinit.d/apache2 restart
sudo apt install phpmyadmin
cd /var/www/html
sudo ln -s /user/share/phpmyadmin
touch .htaccess
sudo nano .htaccess - lisasin read: order allow,deny ja allow from 172.23.13.40 (minu masina ip)

praks4 HHTPS
SSL sert
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt 

koopia
cp /etc/apache2/sites-available/default-ssl.conf /etc/apache2/sites-available/default-ssl.conf.bak

failide kohandamine
nano /etc/apache2/sites-available/default-ssl.conf
	SSLCertificateFile      /etc/ssl/certs/apache-selfsigned.crt
	SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned.key

nano /etc/apache2/sites-available/000-default.conf
	Redirect permanent "/" "https://172.23.13.40/"  (minu masina ip)

a2enmod ssl
a2enmod headers
a2ensite default-ssl

lisaks pidin mina lisama ka ühe rea apache2.conf faili
nano /etc/apache2/apache2.conf
	ServerName localhost


Wordpress
kõigepealt installisin mysql
sudo apt-get install mysql-server
sudo mysql_secure_installation

tegin andmebaasi
	mysql -u root -p
	create database wordpress;
	create user 'user'@'localhost' identified by 'parool';
	grant all on wordpress.* to 'user' identified by 'parool';
	exit;

wordpressi paigaldamine
	wget https://wordpress.org/latest.tar.gz
	tar xpf latest.tar.gz
	rm -rf /var/www/html
	cp -r wordpress /var/www/html
	chown -R www-data:www-data /var/www/html
	
