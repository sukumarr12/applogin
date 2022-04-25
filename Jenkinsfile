pipeline {
    agent any

    stages {
        
        stage('git clone') {
            steps {
                git credentialsId: 'githubid', url: 'https://github.com/sukumarr12/applogin.git'       
            }
        }

        stage('build') {
            steps {
                sh "mvn clean install"
            }
        }

        stage('test') {
            steps {
                echo "this is for testing"
            }
        }

        stage('publish') {
            steps {
                rtUpload (
                    serverId: 'myjfrog',
                    spec: '''
                          {
                              "files": [
                                  {
                                      "pattern": "target/*.war",
                                      "target": "firstrepo/"
                                  }
                              ]
                          }
                    ''',
                    buildName: "${JOB_NAME}",
                    buildNumber: "${BUILD_NUMBER}"
                )
            }
        }

        stage('deploy') {
            steps {
                echo "ansible-playbook -i inventory e2e.yml"
            }
        }
    }

    post {
        always {
            emailext body: '''this is status of

job:"$(JOB_NAME)"
url:"$(BUILD_URL)"''', subject: 'status of "$(JOB_NAME)"', to: 'devops.sukumar@gmail.com'
       }
    }
}
