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
		//withCredentials([usernameColonPassword(credentialsId: 'saidockerhub', variable: 'dockerhub')]) {
		withCredentials([string(credentialsId: 'saidockerpwd', variable: 'saidockerpwd')]) {
		sh "sudo docker login -u saikannepalli -p ${saidockerpwd}"
		sh "sudo docker push saikannepalli/php-redis:${BUILD_ID}"
		sh "sudo docker push saikannepalli/redis-follower:${BUILD_ID}"
		
            }
        }
     } 
	     stage('Deploy to kubbernetes')
	{
	steps{
		        withCredentials([file(credentialsId: 'gcp-key', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
	sh "gcloud auth activate-service-account --key-file=${GOOGLE_APPLICATION_CREDENTIALS}"
//Configuring the project details to Jenkins and communicate with the gke cluster
         sh "gcloud config set project mssdevops-284216"
         sh "gcloud config set compute/zone us-central1-c"
         sh "gcloud config set compute/region us-central1"
         sh "gcloud container clusters get-credentials cluster-1 --zone us-central1-c --project mssdevops-284216"
	 sh "kubectl version"
                  sh "kubectl get ns" 
		  sh "kubectl version" 
			
	    sh "sed -i -e 's,image_to_be_deployed,'saikannepalli/phpredis:${BUILD_ID}',g' frontend-deployment.yaml"
//Kubernetes Deployments and Services
         sh "kubectl apply -f frontend-deployment.yaml"    // This yamal file represent the frontend-deployment 
         sh "kubectl apply -f frontend-service.yaml"
	 sh "sed -i -e 's,image_to_be_deployed,'saikannepalli/redis-follower:${BUILD_ID}',g' redis-follower-deployment.yaml"
	 sh "kubectl apply -f redis-follower-deployment.yaml"
         sh "kubectl apply -f redis-follower-service.yaml"
	 sh "kubectl apply -f redis-leader-deployment.yaml"
	 sh "kubectl apply -f redis-leader-service.yaml"
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

