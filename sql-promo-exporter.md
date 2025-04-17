Installing & Configuring MySQL Prometheus Exporter


Download & Install Prometheus MySQL Exporter

$wget <mySQL Exporter_URL_FROM_OFFICIAL_SITE> 
$tar -xvzf mysqld_exporter*.tar.gz 
$sudo mv mysqld_exporter-*.linux-amd64/mysqld_exporter /usr/local/bin/ 
$sudo chmod +x /usr/local/bin/mysqld_exporter


Create Prometheus Exporter Database User to Access the Database

CREATE USER 'mysqld_exporter'@'<PrometheusHostIP>' IDENTIFIED BY 'StrongPassword';
GRANT PROCESS, REPLICATION CLIENT, SELECT ON *.* TO 'mysqld_exporter'@'<PrometheusHostIP>'; 
FLUSH PRIVILEGES; 
EXIT
