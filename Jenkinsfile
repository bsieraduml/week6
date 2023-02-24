pipeline {
     agent any
     triggers {
          pollSCM('* * * * *')
     }
     stages {
          stage("Compile") {
               steps {
                    sh '''
                    echo 'before chmod'
                    '''
                    sh "chmod +x gradlew"
                    sh '''
                    echo 'before compile java'
                    '''
                    //sh "./gradlew clean"
                    //sh "./gradlew compileJava"
                    sh '''
                    echo 'at the end'
                    '''
               }
          }
     }
}