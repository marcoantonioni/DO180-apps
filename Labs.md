# Elenco dei Labs e requisiti

Dettagli dei singoli labs in: do180-4.2-student-guide.pdf

## Lab: Creating Containerized Services

Performance Checklist
In this lab, you create an Apache HTTP Server container with a custom welcome page.

Outcomes
You should be able to start and customize a container using a container image.

Before You Begin
Open a terminal on workstation as the student user and run the following command:
[student@workstation ~]$ lab container-review start


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


## Managing Images

Performance Checklist
In this lab, you will create and manage container images.

Outcomes
You should be able to create a custom container image and manage container images.

Before You Begin
Open a terminal on workstation as the student user and run the following command:
[student@workstation ~]$ lab image-review start

## Creating Custom Container Images

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


## Deploying Containerized Applications on OpenShift

Performance Checklist
In this lab, you will create an application using the OpenShift Source-to-Image facility.

Outcomes
You should be able to create an OpenShift application and access it through a web browser.

Before You Begin
Open a terminal on workstation as the student user and run the following command:
[student@workstation ~]$ lab openshift-review start


## Deploying Multi-Container Applications

Performance Checklist
In this lab, you will deploy a PHP Application with a MySQL database using an OpenShift
template to define the resources needed by the application.

Outcomes
You should be able to create an OpenShift application comprised of multiple containers and
access it through a web browser.

Before You Begin
Open a terminal on workstation as the student user and run the following commands:
[student@workstation ~]$ lab multicontainer-review start


## Troubleshooting Containerized Applications

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

