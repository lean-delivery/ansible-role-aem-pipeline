aem-pipeline role
=========
[![License](https://img.shields.io/badge/license-Apache-green.svg?style=flat)](https://raw.githubusercontent.com/lean-delivery/ansible-role-aem-pipeline/master/LICENSE)
[![Build Status](https://travis-ci.org/lean-delivery/ansible-role-aem-pipeline.svg?branch=master)](https://travis-ci.org/lean-delivery/ansible-role-aem-pipeline)
[![Build Status](https://gitlab.com/lean-delivery/ansible-role-aem-pipeline/badges/master/build.svg)](https://gitlab.com/lean-delivery/ansible-role-aem-pipeline)
[![Galaxy](https://img.shields.io/badge/galaxy-lean__delivery.aem-pipeline-blue.svg)](https://galaxy.ansible.com/lean_delivery/aem-pipeline)
![Ansible](https://img.shields.io/ansible/role/d/role_id.svg)
![Ansible](https://img.shields.io/badge/dynamic/json.svg?label=min_ansible_version&url=https%3A%2F%2Fgalaxy.ansible.com%2Fapi%2Fv1%2Froles%2Frole_id%2F&query=$.min_ansible_version)

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


Role Variables : default
--------------
Ci/cd variables:

- `git_username`: "User_name"
- `git_email`: "git@email.com"
- `archetypeGitRepository`: 'git@git.epam.com:epm-aem/maven-archetype-2.git'
- `archetypeGitBranch`: master
- `dispatcher_root`: /opt/aemDispatcherCache
- `distr_storage`: /tmp
```yml

env_types_list:
  - dev
  - test
  - uat
  - prod
```
  
Tower server configuration

-`tower_server_url`: https://aem-tower.epm-ldi.projects.epam.com

Sonar server configuration

-`sonar_server_address`: "localhost"
-`sonarPort`: 9000
-`sonar_admin_login`: admin
-`sonar_admin_password`: admin

Maven options:

-`maven_major_version`: 3
-`maven_minor_version`: 6.0
-`maven_home`: /opt/maven
-`maven_link`: "http://ftp.byfly.by/pub/apache.org/maven/maven-3/3.6.0/binaries/apache-maven-3.6.0-bin.tar.gz"
-`maven_package_name`: apache-maven-3.6.0-bin.tar.gz
-`mavenVersion`: '{{ maven_major_version }}.{{ maven_minor_version }}'
-`maven_bin_file`: '{{ maven_home }}/apache-maven-{{ maven_major_version }}.{{ maven_minor_version }}/bin/mvn'

Jenkins configuratin:

```yml
jenkins_plugin_list:
  - ansible
```
-`jenkins_file`: /opt/jenkins.done
```yml
jenkins_package_list:
  - zip
```  
-`jenkins_pip_modules`: ansible
-`jenkins_home`: /var/lib/jenkins
-`jenkins_port`: 8080
-`jenkins_base_link`: 'http://localhost:{{ jenkins_port }}'
-`jenkins_user`: jenkins
```yml
jenkinsAdmin:
  name: jenkins_admin
  password: jenkins_admin
```
-`jenkins_admin_token`: ""
-`jenkins_admin_login`: '{{ jenkinsAdmin.name }}'
```yml
myJenkinsAttributes:
  url: '{{ jenkins_base_link }}'
  url_username: '{{ jenkinsAdmin.name }}'
  url_password: '{{ jenkinsAdmin.password }}'
```
-`jenkinsLoadLink`: '{{ jenkins_base_link }}/login?from=%2F'
-`jenkins_plugin_listLink`: '{{ jenkins_base_link }}/pluginManager/api/xml?depth=1&xpath=/*/*/shortName|/*/*/version&wrapper=plugins'
-`jenkinsPluginInstallLink`: '{{ jenkins_base_link }}/pluginManager/installNecessaryPlugins'
-`jenkinsUpdateCLink`: '{{ jenkins_base_link }}/updateCenter/'
-`jenkinsScriptLink`: '{{ jenkins_base_link }}/script'
-`jenkins_restart_link`: '{{ jenkins_base_link }}/safeRestart'
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
-`CICDVersion`: 2
-`apache_dir`: /etc/httpd
-`ci_aem_author`: "localhost"
-`aem_instance_port`: 4502


Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in
regards to parameters that may need to be set for other roles, or variables that
are used from other roles.

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


Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in
regards to parameters that may need to be set for other roles, or variables that
are used from other roles.

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
