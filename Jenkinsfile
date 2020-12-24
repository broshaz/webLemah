pipeline {
  agent any 
  tools {
    maven 'Maven'
  }
  stages {
    stage ('Initialize') {
      steps {
        sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
            ''' 
      }
    }
    stage ('Software Composition Analysis') {
      steps {
         //sh 'rm owasp*' 
         //sh 'git clone https://github.com/jeremylong/DependencyCheck.git'
         sh 'unzip DependencyCheck.zip'
         sh './dependency-check/bin/dependency-check.sh --scan ./ --enableRetired -f "ALL" -o'
           
       
               }
    }
    stage ('Build') {
      steps {
      sh 'mvn clean package'
       }
    }
    stage ('Deploy-To-Tomcat') {
            steps {
           sshagent(['tomcat']) {
                sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@35.247.137.87:/home/ubuntu/prod/apache-tomcat-8.5.61/webapps/webapp.war'
              }      
           }      
    }  
  }
}
