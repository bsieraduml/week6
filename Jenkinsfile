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
     }
}