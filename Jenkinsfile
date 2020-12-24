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
    
    stage ('Source Composition Analysis') {
      steps {
         sudo sh 'rm owasp* || true'
         sudo sh 'wget "https://raw.githubusercontent.com/broshaz/webLemah/main/owasp-dependency-check.sh" '
         sudo sh 'chmod +x owasp-dependency-check.sh'
         sudo sh 'bash owasp-dependency-check.sh'
         sudo sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
        
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
