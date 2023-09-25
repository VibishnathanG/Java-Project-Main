pipeline {
   environment {
     git_url = "https://github.com/salilkul/java-project.git"
     git_branch = "master"
   }

  agent any
 
  stages {
    stage('Pull Source') {
      steps {
        git credentialsId: 'dab1686c-ccf4-47d7-9eaf-1958262f6303', branch: "${git_branch}", url: "${git_url}"
       
      }
     }
    
    stage('Maven Build') {
     steps { 
          sh "mvn clean package && cp target/*.jar . "
     }
    }
     
     stage('Docker Image Build') {     
        steps {
              sh 'sudo docker build -t myjava-image . '
               }
             }
        stage('Docker image push') {
           steps {
                 withCredentials([usernamePassword(credentialsId: '6b60484c-e184-4668-b9d7-c402672d2a4f', passwordVariable: 'Password', usernameVariable: 'Username')]) {
                 sh "sudo docker login -u ${env.Username} -p ${env.Password}"
                 sh "sudo docker image tag myjava-image salilkul87/myjava-image:test"
                 sh "sudo docker image push salilkul87/myjava-image:test" 
               } 
             }  
          }
//      stage('Deploy app') {
//        steps {
//          sh 'kubectl apply -f app-deploy.yaml'
//         }
      }
    }

//  post {
//    always {
//      deleteDir() /* cleanup the workspace */
//    }
//  }
  }
