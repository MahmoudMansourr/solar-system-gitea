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

    stage('OWASP Dependency Check') {
                steps {
                    dependencyCheck additionalArguments: """
                      --scan '${env.WORKSPACE}'
                      --out '${env.WORKSPACE}/dependency-check-report'
                      --format 'ALL'
                      --prettyPrint
                      --noupdate
                      --failOnError false
                  """, odcInstallation: 'OWASP-DepCheck-10'
                }
            }
    
  }
}
