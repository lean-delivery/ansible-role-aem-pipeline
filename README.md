aem-pipeline role
=========
[![License](https://img.shields.io/badge/license-Apache-green.svg?style=flat)](https://raw.githubusercontent.com/lean-delivery/ansible-role-aem-pipeline/master/LICENSE)
[![Build Status](https://travis-ci.org/lean-delivery/ansible-role-aem-pipeline.svg?branch=master)](https://travis-ci.org/lean-delivery/ansible-role-aem-pipeline)
[![Build Status](https://gitlab.com/lean-delivery/ansible-role-aem-pipeline/badges/master/build.svg)](https://gitlab.com/lean-delivery/ansible-role-aem-pipeline)
[![Galaxy](https://img.shields.io/badge/galaxy-lean__delivery.aem_pipeline-blue.svg)](https://galaxy.ansible.com/lean_delivery/aem_pipeline)
![Ansible](https://img.shields.io/ansible/role/d/39644.svg)
![Ansible](https://img.shields.io/badge/dynamic/json.svg?label=min_ansible_version&url=https%3A%2F%2Fgalaxy.ansible.com%2Fapi%2Fv1%2Froles%2F39644%2F&query=$.min_ansible_version)
=======

AEM  role for Jenkins ci-cd installation 

Requirements
------------

Nodes:
- Sonarqube
- Jenkins
- Minio
- AEM author
- AEM publisher 
- AEM dispatcher

Ansible Modules:
- LDI AEM modules https://github.com/lean-delivery/ansible-modules-aem

Role Variables : default
--------------

Ci/cd variables:

Git user for ci/cd generation

- `git_username`: "User_name"
- `git_email`: "git@email.com"

AEM maven archetype:

- `archetypeGitRepository`: 'git@git.epam.com:epm-aem/maven-archetype-2.git'
- `archetypeGitBranch`: master

for dispatcher archetype template configuration
- `dispatcher_root`: /opt/aemDispatcherCache
- `distr_storage`: /tmp
- `apache_dir`: /etc/httpd

envirenment list for ci/cd generation:
 
```yml
env_types_list:
  - dev
  - test
  - uat
  - prod
```

List of CI/CD projects ,  names options to create  during pipelines geeration.
- gitRepository - empty  repository to create project with sample data and ci/cd.
- autoDeployOn - list of environments and branches for autodeploy configuration

```yml
build_flows:
 -
   project_name: "test"
   gitRepository: ''
   integrationEnv: 'dev'
   autoDeployOn:
    -
      branch: test
      environment: dev,qa
```
Standalone aem author server for test package deployment during build process
- `ci_aem_author`: "localhost"
- `aem_instance_port`: 4502


ansible AWX/Tower server address

-`tower_server_url`: https://aem-tower.ldi.projects.com

Sonar server configuration

- `sonar_server_address`: "localhost"
- `sonarPort`: 9000
- `sonar_admin_login`: admin
- `sonar_admin_password`: admin

Maven options:

- `maven_major_version`: 3
- `maven_minor_version`: 6.0
- `maven_home`: /opt/maven
- `maven_link`: "http://ftp.byfly.by/pub/apache.org/maven/maven-3/3.6.0/binaries/apache-maven-3.6.0-bin.tar.gz"
- `maven_package_name`: apache-maven-3.6.0-bin.tar.gz

Jenkins configuratin:

```yml
jenkins_plugin_list:
  - ansible
```
- `jenkins_file`: /opt/jenkins.done
```yml
jenkins_package_list:
  - zip
```  
- `jenkins_pip_modules`: ansible
- `jenkins_home`: /var/lib/jenkins
- `jenkins_port`: 8080
- `jenkins_base_link`: 'http://localhost:{{ jenkins_port }}'

Jenkins files owner user
- `jenkins_user`: jenkins

```yml
jenkinsAdmin:
  name: jenkins_admin
  password: jenkins_admin
```

- `jenkins_admin_token`: ""
- `jenkins_admin_login`: '{{ jenkinsAdmin.name }}'

```yml
myJenkinsAttributes:
  url: '{{ jenkins_base_link }}'
  url_username: '{{ jenkinsAdmin.name }}'
  url_password: '{{ jenkinsAdmin.password }}'
```

Dependencies
------------

 - Minimal Version of the ansible for installation: 2.7
 - lean_delivery.sonarqube
 - lean_delivery.jenkins
 - lean_delivery.aem-node
 - lean_delivery.aem-dispatcher
 - lean_delivery.minio
 - https://github.com/lean-delivery/ansible-modules-aem




Example Playbook
----------------

```yml
---
- name: "jenkinf ci/cd"
  hosts: jenkins
  roles:
    - role: ansible-role-aem-pipeline
      jenkins_base_link: 'http://{{ inventory_hostname }}:{{ jenkins_port }}'
      aem_version: '6.4'
      aem_instance_port: 4502
      sonar_server_address: "{{ groups['sonar_servers'][0] }}"
      sonarPort: 80
      git_username: "VPupkin"
      git_email: "pupkin@mail.com"
      tower_server_url: https://aem-tower.ldi.projects.com
      archetypeGitRepository: 'git@git.epam.com:VPupkin/maven-archetype-2.git'
      minio_server_address: http://{{ groups['artifact_storage'][0] }}
      minio_port: 9000
      env_types_list:
        - dev
        - test
        - uat
        - prod
      ci_aem_author: "{{ groups['aem_authors'][0] }}"
      sonarJenkinsUser:
        id: 'test_user'
        name: 'Test'

      build_flows:
        - project_name: "petproj1"
          gitRepository: 'git@git.com:VPupkin/aem-cicd-test-fin2.git'
          integrationEnv: 'dev'
          autoDeployOn:
            - branch: develop
              environment: dev


```


License
-------
Apache

Author Information
------------------

authors:
  - Lean Delivery Team <team@lean-delivery.com>
