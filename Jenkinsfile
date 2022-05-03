node{
   stage('SCM Checkout'){
     git 'https://github.com/Gopalaws/gopal-CI-CD-project.git'
   }
   stage('Compile-Package'){

      def mvnHome =  tool name: 'maven7', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
   stage('SonarQube Analysis') {
	        def mvnHome =  tool name: 'maven3', type: 'maven'
	        withSonarQubeEnv('sonar') { 
	          sh "${mvnHome}/bin/mvn sonar:sonar"
	        }
	    }
   stage('Build Docker Imager'){
   sh 'docker build -t gopalsashti/myweb:0.0.2 .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u gopalaws -p ${dockerPassword}"
    }
   sh 'docker push mynew:latest'
   }
   stage('Nexus Image Push'){
   sh "docker login -u admin -p admin123 13.233.192.31:8083"
   sh "docker tag saidamo/myweb:0.0.2 13.233.192.31:8083/damo:1.0.0"
   sh 'docker push 13.233.192.31:8083/damo:1.0.0'
   }
   stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		//  do nothing if there is an exception
	}
   stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest gopalsashti/myweb:0.0.2' 
   }
}
}
