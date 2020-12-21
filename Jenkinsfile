//This line indicates the Application display name
currentBuild.displayName="MiracleGuestBook-#"+currentBuild.number

//This is the Declarative Pipeline Script
pipeline{
    agent any 
     stages{
//Build the Docker Image based on the Dockerfile
        stage('Build Docker Image'){
	  steps{
	     //sh "sudo docker build -t maniengg/php-redis:${BUILD_ID} php-redis/"   //when we run docker in this step, we're running it via a shell on the docker build-pod container
             //sh "sudo docker build -t maniengg/redis-follower:${BUILD_ID} redis-follower/"
            sh "sudo docker build -t saikannepalli/php-redis:${BUILD_ID} php-redis/"		  
            sh "sudo docker build -t saikannepalli/redis-follower:${BUILD_ID} redis-follower/" 
	  }
       }
 // Pushing the Docker Image to DockerHub   
     stage('Push Docker Image'){
	steps{
//Docker Hub Credentials
       // withCredentials([string(credentialsId: 'DOKCER_HUB_PASSWORD', variable: 'DOKCER_HUB_PASSWORD')]) {
	//  sh "sudo docker login -u maniengg -p ${DOKCER_HUB_PASSWORD}"
        //  sh "sudo docker push maniengg/php-redis:${BUILD_ID}"
	  //sh "sudo docker push maniengg/redis-follower:${BUILD_ID}"
	//withCredentials([string(credentialsId: 'saidockerpwd', variable: 'docker-pwd')]) {
		withCredentials([usernameColonPassword(credentialsId: 'saidockerhub', variable: 'dockerhub')]) {
		sh "sudo docker login -u saikannepalli -p ${docker-pwd}"
		sh "sudo docker push saikannepalli/php-redis:${BUILD_ID}"
		sh "sudo docker push saikannepalli/redis-follower:${BUILD_ID}"
		
            }
        }
     }     
// Application Deploying into K8s Cluster
    
 }
// Email Notification
	//post {
       // failure {
          //  script {
            //    currentBuild.result = 'FAILURE'
           // }
        //}

        //always {
         //  step([$class: 'Mailer',notifyEveryUnstableBuild: true,recipients: //"gjilludimudi@miraclesoft.com,pkannepalli@miraclesoft.com,sarikatla@miraclesoft.com,sakapelly@miraclesoft.com,mkarnam@miraclesoft.com",sendToIndividuals: true])
	
        //}
    //}
}

