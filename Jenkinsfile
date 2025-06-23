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

        stage('NPM Dependency Audit') {
                steps {
                    sh '''
                        npm audit --audit-level=critical
                        echo $?
                    '''
                }
            }
        
        stage('OWASP Dependency Check') {
            steps {
                dependencyCheck additionalArguments: """
                    --scan './'
                    --out './'
                    --format 'ALL'
                    --prettyPrint
                    --noupdate
                """, odcInstallation: 'OWASP-DepCheck-10'
                dependencyCheckPublisher failedTotalCritical: 1, pattern: 'dependency-check-report.xml', stopBuild: true

                }
            }
        }
    }

    stage('Unit') { 
        steps { 
            sh 'npm install --no-audit'
        } 
    }
    
  }
}
