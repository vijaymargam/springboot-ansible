pipeline {
  agent any
  tools { 
        maven 'maven'
  }
  stages {
    stage('Clone repository') {
      steps {
        checkout scm
      }
    }
    stage('Build') {
      steps {
      
         sh 'mvn clean package -DskipTests'
      
      } 
    }
    stage('createAnInstance')
    {
      steps {
          //sh 'ansible-playbook createInstance.yaml'
         ansiblePlaybook become: true, credentialsId: 'SudhakarPrivateKey', disableHostKeyChecking: true, inventory: '/etc/ansible/hosts', playbook: '$WORKSPACE/createInstance.yaml'

          sh 'sleep 10'
      }
    }
    
    stage('InstallSoftwares and Deploy code')
    {
      steps {
        
          ansiblePlaybook become: true, colorized: true, credentialsId: 'SudhakarPrivateKey', disableHostKeyChecking: true, extras: '-e WORKSPACE=$WORKSPACE', inventory: '/tmp/hosts', playbook: '$WORKSPACE/deployArtifact.yaml'

          }
    }
    
  }
}
