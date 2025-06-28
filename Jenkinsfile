pipeline {
    agent any
    
    tools {
        nodejs 'nodejs-22-6-0'
    }

    environment {
        MONGO_URI="mongodb://mongodb-service.mongodb.svc.cluster.local:27017/solar-system?authSource=admin"
        MONGO_DB_Creds = credentials('mongo-db-credentials')
        MONGO_USERNAME = credentials('mongo-db-username')
        MONGO_PASSWORD = credentials('mongo-db-password')
    }

    stages {

    stage('Checkout') {
      steps {
        script {
          def scmVars = checkout scm
          env.GIT_COMMIT = scmVars.GIT_COMMIT
        }
        sh "echo ${env.GIT_COMMIT}"
      }
    }
        // stage('Installing Dependencies') {
        //     steps {
        //         sh 'npm install --no-audit'
        //     }
        // }
        
        // stage('Dependency Scanning') {
        //     parallel {
        //         stage('NPM Dependency Audit') {
        //             steps {
        //                 sh '''
        //                     npm audit --audit-level=critical
        //                     echo $?
        //                 '''
        //             }
        //         }
                
        //         stage('OWASP Dependency Check') {
        //             steps {
        //                 dependencyCheck additionalArguments: """
        //                     --scan './'
        //                     --out './'
        //                     --format 'ALL'
        //                     --prettyPrint
        //                     --noupdate
        //                 """, odcInstallation: 'OWASP-DepCheck-10'
        //                 dependencyCheckPublisher failedTotalCritical: 1, pattern: 'dependency-check-report.xml', stopBuild: true
                        
                    
                        
        //             }
        //         }
        //     }
        // }
        
        // stage('Unit Tests') { 
        //     steps { 
        //         withCredentials([usernamePassword(credentialsId: 'mongo-db-credentials', passwordVariable: 'MONGO_PASSWORD', usernameVariable: 'MONGO_USERNAME')]) {
        //             sh '''
        //                 npm test
        //             '''
        //         }
    
        //     } 
        // }

        // stage('Code Coverage') { 
        //     steps { 
        //         withCredentials([usernamePassword(credentialsId: 'mongo-db-credentials', passwordVariable: 'MONGO_PASSWORD', usernameVariable: 'MONGO_USERNAME')]) {
        //             catchError(buildResult: 'SUCCESS', message: 'Oops! we will fix it later', stageResult: 'UNSTABLE') {
        //                 sh 'npm run coverage'
        //             }
        //         }
        //     } 
        // }

        // stage('Build Docker Image') {
        //     steps {
        //         sh 'docker build -t siddharth67/solar-system:$GIT_COMMIT .'
        //     }
        // }
    }

    // post {
    //     always {
    //         junit allowEmptyResults: true, keepProperties: true, testResults: 'dependency-check-junit.xml'
    //         junit allowEmptyResults: true, keepProperties: true, testResults: 'test-results.xml'
    //         publishHTML([allowMissing: true, alwaysLinkToLastBuild: true, keepAll: true, reportDir: 'coverage/lcov-report', reportFiles: 'index.html', reportName: 'Code Coverage HTML Report', reportTitles: '', useWrapperFileDirectly: true])
    //     }
    // } 

}
