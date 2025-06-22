pipeline{
  agent any
  tools {
      nodejs 'nodejs-22-6-0'
  }
  
  
  stages {
      stage('Installing Dependencies') {
          steps {
               sh 'npm install --no-audit'
          }
      }

    stage('Dependency Scanning') {
        parallel {
            
            stage('OWASP Dependency Check') {
                steps {
                    echo "Workspace: ${env.WORKSPACE}"
                    sh "ls -la ${env.WORKSPACE}"
                    
                }
            }
        }
    }
      
  }
}
