def remote = [:]
remote.name = 'Dockerserver'
remote.host = '192.168.1.11'    
remote.allowAnyHosts = true

node {
      stage('Build') {
       def mavenHome = '/opt/maven/bin'       
      sh "${mavenHome}/mvn clean install"
      //archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
   }

    

   stage('Dockerserver'){
    
    withCredentials([usernamePassword(credentialsId: '73c1ef67-741a-448d-9ba7-8cdac27625b3', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]){
        remote.user = USERNAME
        remote.password = PASSWORD
        sshPut remote: remote, from: 'webapp/target/*.war', into: '/opt/docker/'
        //sshCommand remote: remote, command: 'cd /opt/docker; docker stop regappv1; docker rm -f regappv1; docker rmi regapp:v1; docker build -t regapp:v1 .; docker rm -f regappv1; docker run -d --name regappv1 -p 8086:8080 regapp:v1;'
        //sh '''
        //cd /opt/docker
        //docker stop regappv1
        //docker rm -f regappv1
        //docker rmi regapp:v1    
        //'''   
        //docker.build('regapp:v1')
        //sh "docker rm -f regappv1"
        //docker.image('regappv1').withRun('-p 8086:8080 regapp:v1')
        }
    
   }

   
}