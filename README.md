# ETSBDTNDB

<h2>1. Implementasi SQL Cluster</h2>
Implementasi mysql-cluster sama dengan tugas 1, dapat dilihat di <a href="https://github.com/TommyHalim/BDTNDBcluster">sini</a> ditambah dengan penginstalan apache dan wordpress

<h2>2. Instalasi Wordpress</h2>
<h3>a. Install apache</h3>

~~~
sudo apt-get install -y apache2 php libapache2-mod-php php-mysql
sudo apt-get install -y php-gd php-imap php-ldap php-odbc php-pear php-xml php-xmlrpc php-mbstring php-snmp php-soap php-tidy curl
~~~

<h3>b. Copy dan extract wordpress ke dalam vagrant</h3>

~~~
sudo cp '/vagrant/install/wordpress-5.1.1.tar.gz' '/var/www/html/'
cd /var/www/html/
tar -xvf wordpress-5.1.1.tar.gz
~~~

<h3>c. Membuat database di clusterdb2</h3>
Masuk ke mysql clusterdb2

~~~
mysql> CREATE DATABASE wordpress;
mysql> GRANT ALL PRIVILEGES on wordpress.* to 'bdt'@'%';
mysql> FLUSH PRIVILEGES;
~~~

<h3>d. Mengedit wp-config.php pada clusterdb4(ProxySQL) </h3>
Sebelum mengedit wp-config.php rename dahulu wp-config-sample.php ke wp-config.php

~~~
sudo nano wp-config.php
curl -s https://api.wordpress.org/secret-key/1.1/salt/
~~~

'curl -s https://api.wordpress.org/secret-key/1.1/salt/' digunakan untuk mendapatkan secret key autentikasi untuk wordpress, setiap komputer memiliki secret key yang berbeda-beda
Setelah itu ubah menjadi

~~~
/** The name of the database for WordPress */
define( 'DB_NAME', 'wordpress' );

/** MySQL database username */
define( 'DB_USER', 'bdt' );

/** MySQL database password */
define( 'DB_PASSWORD', 'bdt2019' );

/** MySQL hostname */
define( 'DB_HOST', '192.168.33.14:6033' );
.
.
.
.
define('AUTH_KEY',         '50VXOQLRk2yQ}  .$c7O6ksrQuCL2CcNL+ z>TjV%3J86m.WUrCZG[+|;MV+u/C^');
define('SECURE_AUTH_KEY',  '?YMhiMSTNYYhue PpNGI?F]W;5ZcN;DTz8<e_78&N<d5YarRi;Va9a@hpBIOj,KT');
define('LOGGED_IN_KEY',    'J{5,fnP8eamc:bu4zARs1.>Z+Dyv++5T{-|-o4Z(bL/8Y|a:*}dkU Aq1#]d<dAz');
define('NONCE_KEY',        '*~mB4nk9?(16&)sA%t#aXYLb|`Q9 %?S+E9t]Uy#u$(!:wT^pvXdAu:N*si}g)@');
define('AUTH_SALT',        'G*LKAb=^fptkP/;SH7(rL2]1,aIx!8x]yJVx[vmf3tB2ET8)UC{4i~A^{O7UOl&');
define('SECURE_AUTH_SALT', '.Bcc`*)HtAy$3UUv+q}Y}+9qZJ7#WaXmirqmmFHv1/UIx%Z=GEc}% 70lcfNRar|');
define('LOGGED_IN_SALT',   'C?/ce,nnEZxs1qq4|J:;G(=J]Hh=vi. h2gZw$IA^/rODecJ;Z R.K)yw<&BE^R7');
define('NONCE_SALT',       'IfJp<ypHKNEw*0.nU-9&a@}K,-uE0s,+s%d,8b@wjN[{[WrH-`:Wbdr!~-FN!d?Y');
~~~

<h3>e. Ubah database engine menjadi ``ndb`` pada ``wordpress\wp-admin\includes\schema.php`</h3>
Menambahkan setiap table dengan ENGINE=NDB

~~~
sudo nano schema.php
~~~

<img src="">
<br>

<h3>f. Membuka `http://192.168.33.14/wordpress` pada browser</h3>
Lakukan pendaftaran pada wordpress dan ikuti langkah penginstalan
<br>
<img src="">
<br>

<h2>3. Mengetes wordpress</h2>
Membuat post baru dengan mematikan salah satu db (2/3)


<h2>4. Testing dengan JMeter</h2>
