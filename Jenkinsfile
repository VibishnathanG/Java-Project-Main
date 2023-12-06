pipeline {
   environment {
     git_url = "https://github.com/VibishnathanG/Java-Project-Main.git"
     git_branch = "master"
     KUBECONFIG = credentials('a0513d49-7fe4-43e8-aa2f-20c5f5c0a1c2')
   }

  agent any
 
  stages {
    stage('Pull Source') {
      steps {
        git credentialsId: '01e6aded-3b17-43d2-a2f2-82a120a6bb2d', branch: "${git_branch}", url: "${git_url}"
       
      }
     }
    
    stage('Maven Build') {
     steps { 
          sh "mvn clean package && cp target/*.jar . "
     }
    }

     stage('Docker Image Build') {     
        steps {
              sh 'docker build -t myjava-image . '
               }
             }
     
      stage('Docker image push') {
           steps {
                 withCredentials([usernamePassword(credentialsId: '40cae5ac-7ed7-44b1-a204-96331c474bce', passwordVariable: 'Password', usernameVariable: 'Username')]) {
                 sh "docker login -u ${env.Username} -p ${env.Password}"
                 sh "docker image tag myjava-image vibishnathan/myjava-image:test"
                 sh "docker image push vibishnathan/myjava-image:test" 
               } 
             }  
          }
      stage('Deploy app') {
         
          steps {
             script {
                    sh "kubectl --kubeconfig=${KUBECONFIG} apply -f app-deploy.yaml"
                }
         }
   }
    }

  post {
    always {
    deleteDir() /* cleanup the workspace */
    }
 }
  }
