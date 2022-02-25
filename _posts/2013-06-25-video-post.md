---
layout: post
title:  "Deployment Guide"
date:   2022-02-24
excerpt: "How we deployed our website."
tags: [deployment]
comments: true
---
All people must be finished by 8 p.m. the night before the teacher/scrum team review so our Deployment manager has time to deploy by 11:59 p.m. the night before the teacher/scrum team review.

Specs:

       AWS ubuntu 20 EC2 instance 

              -used to host and deploy website 
              url:taxevasion.pentahex.xyz

Plans:

    -Deploy website through the EC2 instance and update it at least once a week


Setup instance:

1. Choose ubuntu 20

2. on choose instance type, choose t2.micro to be free tier eligible

3. use default settings on steps 3-5

4. On configure security group step, add rules for http and https, change ssh "source" to "my ip"

5. hit review and launch, create a new ssh key and download the ssh key

SSH command to access instance:

     open up cmd

     type: ssh -i sshkeypathfile ubuntu@publicipv4dnsofinstance

After successful SSH login:

1. update instance:

        sudo apt update

        sudo apt upgrade

2.downloading java and maven:

        sudo apt install default-jre

        sudo apt install default-jdk

        sudo apt install maven

3.clone, build java web application(building a work in progress currently due to difficulties in code)

        cd

        git clone urlofgithubrepository(in my case it would be https://github.com/wrachel/TaxEvaders.git)

        cd TaxEvaders

        mvn clean install


4.service file for website

       sudo nano /etc/systemd/system/TaxEvaders.service

       copy and paste:

[Unit]

Description=Java

After=network.target

[Service]

User=ubuntu

Restart=always

ExecStart=java -jar /home/ubuntu/nighthawk_csa/target/csa-0.0.1-SNAPSHOT.jar   <---edit this line as appropriate

[Install]

WantedBy=multi-user.target

5.run and enable service file

       sudo systemctl start nighthawk_csa

       systemctl status nightawk_csa
      
       sudo systemctl enable nighthawk_csa

6. enable website to be retrievable by nginx

        sudo nano /etc/nginx/sites-available/TaxEvaders

        server {

           listen 80;

           server_name csa.nighthawkcoders.cf;

           location / {

           proxy_pass http://localhost:8080;

           }

         }

         test configuration, not part of the file:
                 
                     sudo ln -s /etc/nginx/sites-available/TaxEvaders/etc/nginx/sites-enabled

                     sudo nginx -t

7. restart nginx

         sudo systemctl restart nginx


8. use freenom to registor public ip address to domain 

## How to update 
enter machine first

commands:

     cd <repo name>

     git pull <repo url>

     sudo mvn spring-boot:run


  





                     


