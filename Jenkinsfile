pipeline {
     agent any
     triggers {
          pollSCM('* * * * *')
     }
     stages {
          stage("Compile") {
               steps {
                    echo 'Compile'
                    sh "chmod +x gradlew"
                    sh "./gradlew compileJava"
               }
          }
          stage("Unit test") {
               steps {
                    echo 'Unit test'
                    sh "./gradlew test"
               }
          }
          stage("Code coverage") {
               steps {
                    echo 'Code coverage'
                    sh "./gradlew jacocoTestReport"
                    sh "./gradlew jacocoTestCoverageVerification"
               }
          }
          stage("Static code analysis") {
               steps {
                    echo 'Static code analysis'
                    sh "./gradlew checkstyleMain"
               }
          }
          stage("Package") {
               steps {
                    echo 'Package'
                    sh "./gradlew build"
               }
          }

          stage("Docker build") {
               steps {
                    echo 'Docker build'
                    sh "docker build -t leszko/calculator:${BUILD_TIMESTAMP} ."
               }
          }

          stage("Docker login") {
               steps {
                    echo 'Docker login'
                    withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'docker-hub-credentials',
                               usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
                         sh "docker login --username $USERNAME --password $PASSWORD"
                    }
               }
          }

          stage("Docker push") {
               steps {
                    echo 'Docker push'
                    sh "docker push leszko/calculator:${BUILD_TIMESTAMP}"
               }
          }

          stage("Update version") {
               steps {
                    echo 'Update version'
                    sh "sed  -i 's/{{VERSION}}/${BUILD_TIMESTAMP}/g' calculator.yaml"
               }
          }
          
          stage("Deploy to staging") {
               steps {
                    echo 'Deploy to staging'
                    sh "kubectl config use-context staging"
                    sh "kubectl apply -f hazelcast.yaml"
                    sh "kubectl apply -f calculator.yaml"
               }
          }

          stage("Acceptance test") {
               steps {
                    echo 'Acceptance test'
                    sleep 60
                    sh "chmod +x acceptance-test.sh && ./acceptance-test.sh"
               }
          }

          stage("Release") {
               steps {
                    echo 'Release'
                    sh "kubectl config use-context production"
                    sh "kubectl apply -f hazelcast.yaml"
                    sh "kubectl apply -f calculator.yaml"
               }
          }
          stage("Smoke test") {
              steps {
                    echo 'Smoke test'
                  sleep 60
                  sh "chmod +x smoke-test.sh && ./smoke-test.sh"
              }
          }
     }
}