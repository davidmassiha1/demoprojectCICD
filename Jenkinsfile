node {
      stage('Build') {
       def mavenHome = '/opt/maven/bin'       
      sh "${mavenHome}/mvn clean install"
      //archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
   }

   stage('Dockerserver'){
    
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