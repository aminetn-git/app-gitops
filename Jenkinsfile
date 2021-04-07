node {
    
    def project = 'my-project' 
    def imageVersion = 'development' 
    def imageTag = "fekiayman/${project}:${imageVersion}.${env.BUILD_NUMBER}" 
    
    stage("Git Clone"){
        //git credentialsId: 'GIT_CREDENTIALS', url: 'https://github.com/fekiayman/app-gitops.git'
      checkout scm
    }
    
    stage("Maven Clean Build"){
        def mavenHome = tool name: "maven3.6.3", type: "maven"
        def mavenCMD = "${mavenHome}/bin/mvn"
        sh "${mavenCMD} clean package"
       
    }
    
    stage("Build Docker Image"){
        
        sh "cp /var/lib/jenkins/workspace/testjenkinsfile/target/Gestion.war /opt/docker/"
        sh "docker build -t ${imageTag} /opt/docker"
      
    }
    
    stage("Docker Push"){
        
        withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
          sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
          sh "docker push ${imageTag}"
        }
        
    stage('Cleaning up') {
    
        sh "docker rmi ${imageTag}"
    }     
    
        
      
    }
    
    
	}
