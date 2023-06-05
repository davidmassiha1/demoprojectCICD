node {
      stage('Build') {
       def mavenHome = '/opt/maven/bin'       
      sh "${mavenHome}/mvn clean install"
      //archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
   }

   stage('Dockerserver'){
    withCredentials([usernamePassword(credentialsId: '73c1ef67-741a-448d-9ba7-8cdac27625b3', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]){
        sh '''
        cd /opt/docker
        docker stop regappv1
        docker rm -f regappv1
        docker rmi regapp:v1    
        '''   
        docker.build('regapp:v1')
        sh "docker rm -f regappv1"
        docker.image('regappv1').withRun('-p 8086:8080 regapp:v1')
        }
    
   }

   
}