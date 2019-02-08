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
          ansiblePlaybook become: true, credentialsId: 'windows', disableHostKeyChecking: true, inventory: '/etc/ansible/hosts', playbook: '$WORKSPACE/createInstance.yaml'
          sh 'sleep 100'
      }
    }
    
    stage('InstallSoftwares and Deploy code')
    {
      steps {
        
          ansiblePlaybook become: true, colorized: true, credentialsId: 'windows', disableHostKeyChecking: true, extras: '-e WORKSPACE=$WORKSPACE', inventory: '/tmp/hosts', playbook: '$WORKSPACE/deployArtifact.yaml'

          }
    }
    
  }
}
