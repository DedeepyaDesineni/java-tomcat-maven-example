node{
   stage('Checkout'){
      //github step
     git 'https://github.com/rajnikhattarrsinha/java-tomcat-maven-example'
   }
   stage('Build'){
      // Get maven home path and build
      def mvnHome =  tool name: 'Maven 3.5.4', type: 'maven'   
      sh "${mvnHome}/bin/mvn install"
   }
   stage ('Test'){
      //test
      def mvnHome =  tool name: 'Maven 3.5.4', type: 'maven'    
      sh "${mvnHome}/bin/mvn verify; sleep 3"
   
   }  
   /* ----stage('Mvn Package'){
      // Get maven home path
      def mvnHome =  tool name: 'Maven 3.5.4', type: 'maven'   
      sh "${mvnHome}/bin/mvn package"
   } */
   
   stage('Build Docker Image'){
   sh 'docker build -t rajnikhattarrsinha/javatomcat:2.0.0 .'
   }
   
   
   stage('Publish Docker Image')
   {
      withCredentials([string(credentialsId: 'dockerpwd', variable: 'dockerPWD')]) {
    // some block
         sh "docker login -u rajnikhattarrsinha -p ${dockerPWD}"
         }
      sh 'docker push rajnikhattarrsinha/javatomcat:2.0.0'
   
   }
   
   stage('Pull Docker Image and Deploy'){
      def dockerRun= 'sudo docker run -p 8080:8080 -d --name java-tomcat-maven-$BUILD_NUMBER rajnikhattarrsinha/javatomcat:2.0.0'
      sshagent(['dockerdeployserver2']) {
    // some block
         sh 'echo "hello"'
         sh "ssh -o StrictHostKeyChecking=no ubuntu@34.239.128.128 ${dockerRun}"   
                   
      }
   }
   
   
   
}


