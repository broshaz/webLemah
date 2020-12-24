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
         dependencyCheck additionalArguments: ''' 
                    -o "./" 
                    -s "./"
                    --enabledRetired
                    -f "ALL" 
                    --prettyPrint''',
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
