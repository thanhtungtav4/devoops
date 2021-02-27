################# Cài web app server #######################
1. Các dịch vụ yêu cầu:
- Nginx: v1.17.4
- PHP: v7.3

2. Cập nhật OS lên bản mới nhất
	yum update

3. Cài nginx: https://www.cyberciti.biz/faq/how-to-install-and-use-nginx-on-centos-7-rhel-7/
- Tạo nginx repo:
	vi /etc/yum.repos.d/nginx.repo
	
- Copy nội dung vào file nginx repo và save lại:
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/mainline/centos/7/$basearch/
gpgcheck=0
enabled=1

- Chạy lệnh cài đặt:
	yum install nginx
	
- Tự động start php-fpm khi server boot:
	systemctl enable nginx
	
- Start nginx:
	systemctl start nginx
	
**** Cấu hình nginx. (https://www.monitis.com/blog/6-best-practices-for-optimizing-your-nginx-performance/)
a) Mở file /etc/nginx/nginx.conf
- worker_processes: để auto
- thêm vào dòng ở section http: 
	server_tokens off; 

b) Enable Gzip Compression
- Tạo file gzip.conf
	vi /etc/nginx/conf.d/gzip.conf
	
- Copy nội dung sau và lưu lại:
gzip on;
gzip_proxied any;
gzip_types text/plain text/xml text/css application/x-javascript;
gzip_vary on;
gzip_disable "MSIE [1-6]\.(?!.*SV1)";
	
4. Cài đặt php: https://tecadmin.net/install-php7-on-centos7/
- Cài đặt EPEL: 
	yum install epel-release
	
- Cài đặt Remi repository:
	rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
	
- Enable và cài đặt php 7.3
	yum --enablerepo=remi-php73 install php
	
- Cài đặt module cần thiết cho php
	yum --enablerepo=remi-php73 install php-fpm php-mysql php-pgsql php-sqlite php-pecl-memcache php-pecl-memcached php-gd php-mbstring php-mcrypt php-xml php-pecl-apc php-cli php-pear php-pdo php-soap
	
- Tự động start php-fpm khi server boot:
	systemctl enable php-fpm
	
- Start php-fpm:
	systemctl start php-fpm
	
****** Cấu hình php 
a) Mở file /etc/php.ini
- Tắt php version: tìm đến dòng expose_php. và chỉnh lại thành off
	expose_php = off
	
b) Mở file /etc/php-fpm.d/www.conf 
- Chỉnh user và group về từ apache thành nginx 
	; RPM: apache user chosen to provide access to the same directories as httpd
	user = nginx
	; RPM: Keep a group allowed to write in log dir.
	group = nginx
	
5. Mở port cho nginx: (https://www.rootusers.com/how-to-open-a-port-in-centos-7-with-firewalld/)
- Kiểm tra các port đang mở:
	firewall-cmd --list-ports
	
- Mở port 80 và 443
	firewall-cmd --permanent --add-port=80/tcp
	firewall-cmd --permanent --add-port=443/tcp
	firewall-cmd --reload
	
6. cài đặt git
sudo yum install git
7. cài đặt compomser
- sudo curl -s https://getcomposer.org/installer | php
- sudo mv composer.phar /usr/local/bin/composer
8. cai dat unzip
yum install zip unzip php-zip

9. cai yum-utils
yum update && yum install yum-utils
https://stackoverflow.com/questions/49583881/how-can-i-install-ziparchive-on-php-7-3-with-centos-7
yum-config-manager --enable remi-php72
yum install php-pecl-zip


install php extention zip
yum install php-pecl-zip
