##############truocw taoj use ssh
http://dvms.vn/tin-tuc/tin-nganh/419-huong-dan-tao-webserver-voi-google-cloud-free-cai-dat-kloxo-len-google-cloud.html
https://tuts-linux.blogspot.com/2014/10/tao-mat-khau-cho-root-trong-ubuntu.html
################# Cài database server #######################
1. Các dịch vụ yêu cầu:
- MariaDB: v10.4

2. Cập nhật OS lên bản mới nhất
	yum update
	
3. Cài MariaDB: https://tecadmin.net/install-mariadb-10-centos-redhat/
- Tạo file mariadb repo:
	vi /etc/yum.repos.d/mariadb.repo
	
- Copy nội dung sau vào file và lưu lại:
[mariadb]
name = MariaDB
baseurl = http://yum.mariadb.org/10.4/rhel7-amd64
gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
gpgcheck=1

- Chạy lệnh cài đặt:
	yum install MariaDB-server MariaDB-client -y
	
- Tự động start MariaDB khi server boot:
	systemctl enable mariadb
	
- Start MariaDB:
	systemctl start mariadb
	
4. Cấu hình bảo mật cho MariaDB
- Chạy lệnh:
	mysql_secure_installation
	
- Pass root: nISbBPnn3bPzC66fGk3S
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'nISbBPnn3bPzC66fGk3S' WITH GRANT OPTION
GRANT ALL PRIVILEGES ON kms.* TO 'kms'@'%' IDENTIFIED BY 'Kms@123456' WITH GRANT OPTION


5. Mở port cho app server (https://vinasupport.com/huong-dan-mo-cong-port-tren-centos-7/)
- Kiểm tra port đang mở:
	firewall-cmd --list-ports
	
- Mở port 3306:
	firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="27.71.225.210/32" port protocol="tcp" port="3306" accept'
	firewall-cmd --reload
	
	###UPDATE mysql.user SET Password=PASSWORD('Kms@123456') WHERE User='root'; FLUSH PRIVILEGES; exit;
	


