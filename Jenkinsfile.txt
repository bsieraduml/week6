pipeline {
     agent any
     triggers {
          pollSCM('* * * * *')
     }
     stages {
          stage("Compile") {
               steps {
                   dir("/week6_exercise/chapter08/sample1")
                   {
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
}