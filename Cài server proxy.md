################# Cài server proxy #######################
1. Các dịch vụ yêu cầu:
- Nginx: v1.17.4
- Squid: v3.5.20

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

4. Cài đặt Squid proxy server: (https://hostpresto.com/community/tutorials/how-to-install-and-configure-squid-proxy-on-centos-7/)
- Chạy lệnh cài đặt:
	yum install squid
	
- Tự động start squid khi server boot:
	systemctl enable squid
	
- Start nginx:
	systemctl start squid
	
- Cài đặt basic auth cho squid:
	yum -y install httpd-tools
	
- Tạo file basic auth:
	touch /etc/squid/passwd && chown squid /etc/squid/passwd
	
- Tạo user authen và nhập password authen:
	htpasswd /etc/squid/passwd vgis
	
**** Cấu hình cho squid
- Cấu hình authen. Mở file: /etc/squid/squid.conf và thêm vào nội dung sau phần list ports:
	auth_param basic program /usr/lib64/squid/basic_ncsa_auth /etc/squid/passwd
    auth_param basic children 5
    auth_param basic realm Squid Basic Authentication
    auth_param basic credentialsttl 2 hours
    acl auth_users proxy_auth REQUIRED
    http_access allow auth_users
	
- Đổi port proxy server. Mở file /etc/squid/squid.conf và tìm đến dòng http_port sửa thành port 8080
	# Squid normally listens to port 3128
    http_port 8080
	
=> Restart lại squid để apply
	systemctl restart squid

5. Mở port: (https://www.rootusers.com/how-to-open-a-port-in-centos-7-with-firewalld/)
- Kiểm tra các port đang mở:
	firewall-cmd --list-ports
	
- Mở port 80 và 443 cho nginx
	firewall-cmd --permanent --add-port=80/tcp
	firewall-cmd --permanent --add-port=443/tcp
	firewall-cmd --reload
	
- Mở port cho squid
	firewall-cmd --permanent --add-port=8080/tcp
	firewall-cmd --reload
	
6. Sử dụng proxy.
- Cấu hình proxy cho trình duyệt với ip: http://27.71.225.211:8080 
- Đăng nhập với thông tin: vgis/IntelliVgis123
7. cài đặt SSL :  (https://linode.com/docs/security/ssl/install-lets-encrypt-to-create-ssl-certificates/)
sudo yum update && sudo yum upgrade
sudo yum install git
sudo git clone https://github.com/letsencrypt/letsencrypt /opt/letsencrypt
cd /opt/letsencrypt
sudo -H ./letsencrypt-auto certonly --standalone -d example.com -d www.example.com

chmod +x /home/
chmod +x /home/username
chmod +x /home/username/siteroot

https://stackoverflow.com/questions/6795350/nginx-403-forbidden-for-all-files
