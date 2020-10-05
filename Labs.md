# Elenco dei Labs

Dettagli dei singoli labs in: do180-4.2-student-guide.pdf

## Lab: Creating Containerized Services

Performance Checklist
In this lab, you create an Apache HTTP Server container with a custom welcome page.

Outcomes
You should be able to start and customize a container using a container image.

Before You Begin
Open a terminal on workstation as the student user and run the following command:

[student@workstation ~]$ lab container-review start

```bash
sudo podman run -d -p 8080:80 --name httpd-basic redhattraining/httpd-parent:2.4
sudo podman ps
curl http://localhost:8080
sudo podman exec -it httpd-basic /bin/bash
echo "Hello World" > /var/www/html/index.html
exit
curl http://localhost:8080
```

## Lab: Managing Containers

Performance Checklist
In this lab, you will deploy a container that saves the MySQL database data into a host folder,
loads the database, and manages the container.

Outcomes
You should be able to deploy and manage a persistent database using a shared volume. You
should also be able to start a second database using the same shared volume and observe
that the data is consistent between the two containers because they are using the same
directory on the host to store the MySQL data.

Before You Begin
Open a terminal on workstation as the student user and run the following command:

[student@workstation ~]$ lab manage-review start

```bash
sudo mkdir -p /var/local/mysql
sudo semanage fcontext -a -t container_file_t "/var/local/mysql(/.*)?"
sudo restorecon -R -v /var/local/mysql

sudo podman pull rhscl/mysql-57-rhel7
sudo podman inspect rhscl/mysql-57-rhel7 | grep -i user

sudo useradd mysql -u <valore-da-grep>
sudo chown -R mysql:mysql /var/local/mysql

sudo podman run -d --name mysql-1 -v /var/local/mysql:/var/lib/mysql/data -e MYSQL_USER=user1 -e MYSQL_PASSWORD=mypa55 -e MYSQL_DATABASE=items -e MYSQL_ROOT_PASSWORD=r00tpa55 rhscl/mysql-57-rhel7

cat /home/student/DO180/labs/manage-review/db.sql

sudo podman inspect myqsl-1 | grep -i ipaddress

mysql -uuser1 -h 10.88.100.114 > -pmypa55 items < /home/student/DO180/labs/manage-review/db.sql

# oppure
  mysql -uuser1 -pmypa55 -h 10.88.100.114 items

  CREATE TABLE Projects (id int(11) NOT NULL, name varchar(255) DEFAULT NULL, code varchar(255) DEFAULT NULL, PRIMARY KEY (id));
  INSERT into Projects (id, name, code) values (1,'DevOps','DO180');
  exit

sudo podman exec mysql-1 /bin/bash -c 'mysql -uuser1 -pmypa55 items -e "select * from Projects;"'

sudo podman stop mysql-1

sudo podman run -d --name mysql-2 -p 13306:3306 -v /var/local/mysql:/var/lib/mysql/data -e MYSQL_USER=user1 -e MYSQL_PASSWORD=mypa55 -e MYSQL_DATABASE=items -e MYSQL_ROOT_PASSWORD=r00tpa55 rhscl/mysql-57-rhel7

sudo podman ps -all
sudo podman ps -all > /tmp/my-containers

mysql -uuser1 -h workstation.lab.example.com -pmypa55 -P13306 items

insert into Item (description, done) values ('Finished lab', 1);
exit

sudo podman rm -f mysql-2
```


## Lab: Managing Images

Performance Checklist
In this lab, you will create and manage container images.

Outcomes
You should be able to create a custom container image and manage container images.

Before You Begin
Open a terminal on workstation as the student user and run the following command:

[student@workstation ~]$ lab image-review start

```bash
sudo podman search docker.io/nginx

sudo podman pull docker.io/library/nginx
sudo podman images

sudo podman run -d -p 8080:80 --name official-nginx nginx 
sudo podman exec -it official-nginx /bin/bash
echo "DO180" > /usr/share/nginx/html/index.html
exit

curl localhost:8080

sudo podman stop official-nginx
sudo podman commit official-nginx do180/mynginx:v1.0-SNAPSHOT


sudo podman run -d -p 8080:80 --name official-nginx-dev do180/mynginx:v1.0-SNAPSHOT 
sudo podman exec -it official-nginx-dev /bin/bash
echo "DO180 Page" > /usr/share/nginx/html/index.html
exit

curl localhost:8080


sudo podman stop official-nginx-dev
sudo podman commit official-nginx-dev do180/mynginx:v1.0

sudo podman rmi -f do180/mynginx:v1.0-SNAPSHOT

sudo podman run -d -p 8280:80 --name my-nginx do180/mynginx:v1.0

curl localhost:8280
```

## Lab: Creating Custom Container Images

Performance Checklist
In this lab, you will create a Dockerfile to build a custom Apache Web Server container image.
The custom image will be based on a RHEL 7.7 UBI image and serve a custom index.html
page.

Outcomes
You should be able to create a custom Apache Web Server container that hosts static HTML
files.

Before You Begin
Open a terminal on workstation as the student user and run the following command:

[student@workstation ~]$ lab dockerfile-review start

```bash
vi /home/student/DO180/labs/dockerfile-review/Dockerfile

FROM ubi7/ubi:7.7
MAINTAINER marco marco@home.net
ENV PORT 8080
RUN yum -y install httpd && yum -y clean all
EXPOSE $PORT
COPY ./src/* /var/www/html/
CMD ["httpd","-D","FOREGROUND"]


cd /home/student/DO180/labs/dockerfile-review/
sudo podman build -t do180/custom-apache .

sudo podman run -d -p 20080:8080 --name dockerfile do180/custom-apache
curl localhost:20080

```


## Lab: Deploying Containerized Applications on OpenShift

Performance Checklist
In this lab, you will create an application using the OpenShift Source-to-Image facility.

Outcomes
You should be able to create an OpenShift application and access it through a web browser.

Before You Begin
Open a terminal on workstation as the student user and run the following command:

[student@workstation ~]$ lab openshift-review start

```bash
source /usr/local/etc/ocp4.config
oc login ...

oc new-project ${RHT_OCP4_DEV_USER}-ocp

oc new-app --name=temps --context-dir=temps -i php:7.1 https://github.com/RedHatTraining/DO180-apps

oc expose service temps

curl http://temps-${RHT_OCP4_DEV_USER}-ocp.${RHT_OCP4_WILDCARD_DOMAIN}
```


## Lab: Deploying Multi-Container Applications

Performance Checklist
In this lab, you will deploy a PHP Application with a MySQL database using an OpenShift
template to define the resources needed by the application.

Outcomes
You should be able to create an OpenShift application comprised of multiple containers and
access it through a web browser.

Before You Begin
Open a terminal on workstation as the student user and run the following commands:

[student@workstation ~]$ lab multicontainer-review start

```bash
cd ~/DO180/labs/multicontainer-review

cd ./images/mysql
./build.sh
cd ../..
cd ./images/quote-php
./build.sh

sudo podman images | grep do180

sudo podman login -u ${RHT_OCP4_QUAY_USER} quay.io 

sudo podman tag do180/mysql-57-rhel7 quay.io/marco_antonioni/mysql
sudo podman push quay.io/marco_antonioni/mysql-57-rhel7 

sudo podman tag do180/quote-php quay.io/marco_antonioni/quote-php 
sudo podman push quay.io/marco_antonioni/quote-php

# rendere public le due immagini su sito web https://quay.io/repository/

cd /home/student/DO180/labs/multicontainer-review/
cat quote-php-template.json

oc new-project ${RHT_OCP4_DEV_USER}-deploy

oc process --parameters -f quote-php-template.json
oc process -p=RHT_OCP4_QUAY_USER=${RHT_OCP4_QUAY_USER} -f quote-php-template.json | oc create -f -

oc expose service quote-php
oc get route

curl http://quote-php-${RHT_OCP4_DEV_USER}-deploy.${RHT_OCP4_WILDCARD_DOMAIN}
```


## Lab: Troubleshooting Containerized Applications

Performance Checklist
In this lab, you will troubleshoot the OpenShift build and deployment process for a Node.js
application.

Outcomes
You should be able to identify and solve the problems raised during the build and
deployment process of a Node.js application.

Before You Begin
A running OpenShift cluster.
Open a terminal on workstation as the student user and run the following command:

[student@workstation ~]$ lab troubleshoot-review start

```bash
```

