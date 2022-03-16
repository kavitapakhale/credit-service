node 
{ 
     stage('checkout')
  {
     checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/kavitapakhale/credit-service']]])
  }
stage ('Initial Setup')
     {
          sh 'mvn clean compile'
     }


}
