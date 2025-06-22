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
                    def scanPath = env.WORKSPACE
                    def outPath = "${scanPath}/dependency-check-report"

                    dependencyCheck additionalArguments: '''
                        --scan "${scanPath}"
                        --out "${outPath}"
                        --format 'ALL'
                        --prettyPrint
                    ''', odcInstallation: 'OWASP-DepCheck-10'
                }
            }
        }
    }
      
  }
}
