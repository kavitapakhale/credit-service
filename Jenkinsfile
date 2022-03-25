podTemplate(cloud: 'kubernetes',label: 'kubernetes',
            containers: [
                    containerTemplate(name: 'podman', image: 'quay.io/containers/podman', privileged: true, command: 'cat', ttyEnabled: true)
					
            ]) 
{
node 
{ 
   def MAVEN_HOME = tool "mymaven"
   env.PATH = "${env.PATH}:${MAVEN_HOME}/bin"

     stage('checkout')
  {
     checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/kavitapakhale/credit-service.git']]])
  }
stage ('Initial Setup')
     {
          sh 'mvn clean compile'
     }
stage ('Unit Testing')
   {
   sh 'mvn test'
   }
stage('Code Quality Analysis'){
    
    /*withSonarQubeEnv('kaviprash') 
	    	{
                 sh 'mvn sonar:sonar -Dsonar.organization=kaviprash -Dsonar.projectKey=kaviprash'
		
    		}*/
	
  }
stage('code build'){
	sh 'mvn package'
	
	stash includes: 'Dockerfile', name: 'dfile'
        stash includes: 'target/', name: 'efile'
	}
}
node('kubernetes'){
   container('podman') {
	stage('Image Build'){
	   unstash 'dfile'
	   unstash 'efile'
	   sh 'podman image build -t kaviprash-credit-service .'	
		
	}
   }
}
node{
	stage('Dev Deploy'){
			
			sh '/tmp/kubectl apply -f k8s/Deployment.yaml'
			
		}
		
	}
}
