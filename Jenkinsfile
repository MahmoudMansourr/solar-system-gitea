pipeline {
         agent {
        kubernetes {
            yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: nodejs
    image: node:18-alpine3.17
    command:
    - cat
    tty: true
  - name: kaniko
    image: gcr.io/kaniko-project/executor:debug
    command:
      - sleep
    args:
    - 99d
    volumeMounts:
    - name: kaniko-secret
      mountPath: /kaniko/.docker
  - name: git
    image: alpine/git:latest
    command:
      - sleep
    args:
    - 99d    
  - name: trivy
    image: aquasec/trivy:latest
    command:
      - sleep
    args:
    - 99d
    volumeMounts:
    - name: kaniko-secret
      mountPath: /root/.docker    
  volumes:
  - name: kaniko-secret
    secret:
      secretName: regcred
"""
        }
    }
    
    tools {
        nodejs 'nodejs-22-6-0'
    }

    environment {
        MONGO_URI="mongodb://mongodb-service.mongodb.svc.cluster.local:27017/solar-system?authSource=admin"
        MONGO_DB_Creds = credentials('mongo-db-credentials')
        MONGO_USERNAME = credentials('mongo-db-username')
        MONGO_PASSWORD = credentials('mongo-db-password')
       // SONAR_SCANNER_HOME = tool 'sonarqube-scanner-610'
    }

    stages {

    // stage('Checkout') {
    //   steps {
    //     script {
    //       def scmVars = checkout scm
    //       env.GIT_COMMIT = scmVars.GIT_COMMIT
    //     }
    //   }
    // }

    // stage('env variables') {
    //   steps {
    //     sh "echo ${GIT_COMMIT}"
    //     sh "echo ${GIT_BRANCH}"
    //   }
    // }

        stage('Installing Dependencies') {
            steps {
                container('nodejs'){
                    sh 'ls -la'
                    sh 'npm install --no-audit'
                }
            }
        }
        
        // stage('Dependency Scanning') {
        //     parallel {
        //         stage('NPM Dependency Audit') {
        //             steps {
        //                 container('nodejs'){
        //                     sh '''
        //                         npm audit --audit-level=critical
        //                         echo $?
        //                     '''
        //                 }
        //             }
        //         }
                
        //         // stage('OWASP Dependency Check') {
        //         //     steps {
        //         //         dependencyCheck additionalArguments: """
        //         //             --scan './'
        //         //             --out './'
        //         //             --format 'ALL'
        //         //             --prettyPrint
        //         //             --disableYarnAudit
        //         //             --noupdate
        //         //         """, odcInstallation: 'OWASP-DepCheck-10'
        //         //         dependencyCheckPublisher failedTotalCritical: 1, pattern: 'dependency-check-report.xml', stopBuild: true
                            
        //         //     }
        //         // }
        //     }
        // }
        
        // stage('Unit Tests') { 
        //     steps { 
        //         container('nodejs'){
        //             withCredentials([usernamePassword(credentialsId: 'mongo-db-credentials', passwordVariable: 'MONGO_PASSWORD', usernameVariable: 'MONGO_USERNAME')]) {
        //                 sh '''
        //                     npm test
        //                 '''
        //             }
        //         }
    
        //     } 
        // }

        // stage('Code Coverage') { 
        //     steps { 
        //         container('nodejs'){

        //         withCredentials([usernamePassword(credentialsId: 'mongo-db-credentials', passwordVariable: 'MONGO_PASSWORD', usernameVariable: 'MONGO_USERNAME')]) {
        //             catchError(buildResult: 'SUCCESS', message: 'Oops! we will fix it later', stageResult: 'UNSTABLE') {
        //                 sh 'npm run coverage'
        //             }
        //             }
        //         }
        //     } 
        // }
        // stage('SonarQube Analysis') {

        //     steps {
        //             withSonarQubeEnv('sonarqube-server') {
        //                 sh 'echo $SONAR_SCANNER_HOME'
        //                 sh '''
        //                     $SONAR_SCANNER_HOME/bin/sonar-scanner \
        //                     -Dsonar.projectKey=mahmoudmansourr-solar-system \
        //                     -Dsonar.sources=app.js \
        //                     -Dsonar.javascript.lcov.reportPaths=./coverage/lcov.info
        //             '''
        //             }
        //     }
        // }
        stage('Build and Push Docker Image') {
            steps {
                container('kaniko') {
                      sh """
                    /kaniko/executor \
                    --dockerfile=Dockerfile \
                    --context=`pwd` \
                    --destination=docker.io/mahmoudmansourr/solar-app:latest
                    --no-push
                    """
                }
            }
        }

    }

    // post {
    //     always {
    //         junit allowEmptyResults: true, keepProperties: true, testResults: 'dependency-check-junit.xml'
    //         junit allowEmptyResults: true, keepProperties: true, testResults: 'test-results.xml'
    //         publishHTML([allowMissing: true, alwaysLinkToLastBuild: true, keepAll: true, reportDir: 'coverage/lcov-report', reportFiles: 'index.html', reportName: 'Code Coverage HTML Report', reportTitles: '', useWrapperFileDirectly: true])
    //     }
    // } 

}
