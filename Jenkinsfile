pipeline {
    agent any

    stages {
        /*stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        } */
        stage ('Test') {
               agent{
                  docker {
                      image 'node:18-alpine'
                      reuseNode true
                  }
               }
               steps{
                  sh '''
                       test -f build/index.html
                       npm test
                  ''' 
               }  
        }
        stage ('E2E') {
               agent{
                  docker {
                      image 'mcr.microsoft.com/playwright:v1.47.2-noble'
                      reuseNode true
                      // args 'u root:root' running as root user is not advisable
                  }
               }
               steps{
                    // running npm as local and build 'serve'
                  sh '''
                      npm install serve
                      node_modules/.bin/serve -s build
                      npx playwright test
                  ''' 
               }  
        }
        
    }
    post{
            always {
                 junit 'test-results/junit.xml'
            }
        }
}