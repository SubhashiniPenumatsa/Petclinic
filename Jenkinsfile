pipeline {
  agent any
  stages {
    
    stage('Build Api') {
      steps {
        sh 'cd spring-petclinic-rest && nohup mvn spring-boot:run &'
        sleep(20)
      }
    } 
    stage('Build Angular') {
      steps {
        sh 'cd spring-petclinic-angular/static-content && curl https://jcenter.bintray.com/com/athaydes/rawhttp/rawhttp-cli/1.0/rawhttp-cli-1.0-all.jar -o rawhttp.jar && nohup java -jar ./rawhttp.jar serve . -p 4200 &'
        sleep(3)
      }
     
  }
     stage('Robot') {
            steps {
                sh 'robot --variable BROWSER:headlesschrome -d spring-petclinic-angular/Tests/Results spring-petclinic-angular/Tests'
               
                
            }
            post {
                always {
                    script {
                        step(
                            [
                                $class                  :   'RobotPublisher',
                                outputPath              :   'spring-petclinic-angular/Tests/Results',
                                outputFileName          :   '**/output.xml',
                                reportFileName          :   '**/report.html',
                                logFileName             :   '**/log.html',
                                disableArchiveOutput    :   false,
                                passThreshold           :   100,
                                unstableThreshold       :   40,
                                otherFiles              :   "**/*.png,**/*.jpg",
                            ]
                        )
                    }
                }
            }
     }
    stage('RobotEdit') {
            steps {
                sh 'robot --variable BROWSER:headlesschrome -d Robotframework-edit/Results Robotframework-edit/Tests'
               
                
            }
            post {
                always {
                    script {
                        step(
                            [
                                $class                  :   'RobotPublisher',
                                outputPath              :   'Robotframework-edit/Results',
                                outputFileName          :   '**/output1.xml',
                                reportFileName          :   '**/report1.html',
                                logFileName             :   '**/log1.html',
                                disableArchiveOutput    :   false,
                                passThreshold           :   100,
                                unstableThreshold       :   40,
                                otherFiles              :   "**/*.png,**/*.jpg",
                            ]
                        )
                    }
                }
            }
     }   
   
   
    stage('newman') {
            steps {
                sh 'newman run  --environment --reporters junit'
            }
            post {
                always {
                        junit '**/*xml'
                    }
                }
        }
 }
}
