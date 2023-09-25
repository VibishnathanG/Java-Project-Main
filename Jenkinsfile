pipeline {
   environment {
     git_url = "https://github.com/VibishnathanG/Java-Project-Main.git"
     git_branch = "master"
   }

  agent any
 
  stages {
    stage('Pull Source') {
      steps {
        git credentialsId: '1b4c7501-74c0-4d21-b8a0-5e9e68a096b8', branch: "${git_branch}", url: "${git_url}"
       
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
                 withCredentials([usernamePassword(credentialsId: '40cae5ac-7ed7-44b1-a204-96331c474bce', passwordVariable: 'Password', usernameVariable: 'Username')]) {
                 // sh "sudo docker login -u ${env.Username} -p ${env.Password}"
                 sh "echo ${env.Password} | docker login -u ${env.Username} --password-stdin"
                 sh "sudo docker image tag myjava-image vibish/myjava-image:test"
                 sh "sudo docker image push vibish/myjava-image:test" 
               } 
             }  
          }
//      stage('Deploy app') {
//        steps {
//          sh 'kubectl apply -f app-deploy.yaml'
//         }
//    }
    }

//  post {
//    always {
//      deleteDir() /* cleanup the workspace */
//    }
//  }
  }
