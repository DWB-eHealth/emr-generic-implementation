# emr-generic-implementation
For testing, training and showcase purposes

## Deployment steps  
* Install Zlib as a pre-requirement  
```$ yum install -y https://kojipkgs.fedoraproject.org//packages/zlib/1.2.11/19.fc30/x86_64/zlib-1.2.11-19.fc30.x86_64.rpm```

* Get Bahmni 0.92 installer  
```$ yum install -y https://dl.bintray.com/bahmni/rpm/rpms/bahmni-installer-0.92-155.noarch.rpm```

* Get the bahmni configuration files  
```$ cd /etc/bahmni-installer/deployment-artifacts/ && wget https://github.com/DWB-eHealth/emr-generic-implementation/archive/master.zip && unzip master.zip && mv emr-generic-implementation emr-generic-implementation && rm master.zip```

* Get the /etc/bahmni-installer/setup.yml with Dhaka timezone and implementation_name: emr-generic-implementation cp /etc/bahmni-installer/deployment-artifacts/emr-generic-implementation/etc/bahmni-installer/setup.yml /etc/bahmni-installer/```

* Get the inventory file /etc/bahmni-installer/prod with bahmni-emr, bahmni-emr-db, bahmni-reports, bahmni-reports-db  
```$ cp /etc/bahmni-installer/deployment-artifacts/emr-generic-implementation/etc/bahmni-installer/prod /etc/bahmni-installer/```

* Get the latest database snapshot  
```$ cp /etc/bahmni-installer/deployment-artifacts/emr-generic-implementation/database-snapshots/latest-openmrs_backup.sql /etc/bahmni-installer/deployment-artifacts/openmrs_backup.sql```

* Run the installer  
```$ bahmni -i prod install```

* Load Initializer configuration files - Optional if database snapshot is imported  
```$ sudo ln -s  /var/www/bahmni_config/configuration/ /opt/openmrs/configuration```

* Install the modules and restart OpenMRS using the playbooks  
```$ ansible-playbook /var/www/bahmni_config/playbooks/all.yml -i /etc/bahmni-installer/prod --extra-vars '@/var/www/bahmni_config/playbooks/setup.yml'```

### Appointments scheduling V2 setup  
* The installation of the Appointment scheduling V2 (OMOD module and frontend UI) are deployed through the playbooks. The only manual step to be completed across environments is the HTTPD alias addition in `/etc/httpd/conf.d/ssl.conf`  
`Alias /appointments-v2 /var/www/appointments`  
`$ systemctl restart httpd`  
