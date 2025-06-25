pipeline {
    agent any
    tools {
        nodejs 'nodejs-22-6-0'
    }
    environment {
         MONGO_URI="mongodb://mongodb-service.mongodb.svc.cluster.local:27017/solar-system?authSource=admin"
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
                        junit allowEmptyResults: true, keepProperties: true, testResults: 'dependency-check-junit.xml'
                    
                        publishHTML(
                          allowMissing: true,
                          alwaysLinkToLastBuild: true,
                          keepAll: true,
                          reportDir: '.',
                          reportFiles: 'dependency-check-jenkins.html',
                          reportName: 'Dependency Check HTML',
                          reportTitles: '',
                          useWrapperFileDirectly: true
                          )
                    }
                }
            }
        }
        
        stage('Unit Tests') { 
            steps { 
                withCredentials([usernamePassword(credentialsId: 'mongo-db-credentials', passwordVariable: 'MONGO_PASSWORD', usernameVariable: 'MONGO_USERNAME')]) {
                    sh '''
                        npm test
                    '''
                }
                junit allowEmptyResults: true, keepProperties: true, testResults: 'test-results.xml'
            } 
        }
    }
}
