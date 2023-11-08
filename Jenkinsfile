pipeline {
  agent any

  stages {
    stage('Ansible - checkout SCM') {
      steps
      {
        sh("rm -rf *")
      }
    }
    stage('Ansible - checkout SCM') {
      steps {
        dir ('ansible') {
          //git branch: 'main', credentialsId: 'github-snow', url: 'git@github.com:hhai-le/snow-ansible.git'
          //checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github-snow', url: 'git@github.com:hhai-le/snow-ansible. git']])
          checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github-snow', url: 'git@github.com:hhai-le/snow-ansible.git']])
        }
      }
    }
    stage('Terraform - Provising on vSphere') {
      steps {
        dir ('terraform') {
          checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github-snow', url: 'git@github.com:hhai-le/snow-terraform-1-zabbix-server.git']])
          sh """
          #!/bin/bash
          terraform init
          terraform apply -auto-approve  -var-file=ubuntu.tfvars -var='vsphere_virtual_machine_name=${vsphere_virtual_machine_name}' -var='linux_vm_hostname=${linux_vm_hostname}' -var='ipv4_address=${ipv4_address}' -var='server_roles=[${server_roles}]'
          """
        }
      }
    }
  }
}
