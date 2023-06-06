def remote = [:]
remote.name = 'Dockerserver'
remote.host = '192.168.1.10'    
remote.allowAnyHosts = true


node {
      stage('Build') {
       def mavenHome = '/opt/maven/bin'       
      sh "${mavenHome}/mvn clean install"
      //archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
   }

    

   stage('CI Stage'){
    
    withCredentials([usernamePassword(credentialsId: 'b796eb85-7139-459c-bd32-6db10d192339', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]){
        def workspace = env.WORKSPACE
        remote.user = USERNAME
        remote.password = PASSWORD
        echo USERNAME
        echo workspace
        sh "sudo -S cp ${workspace}/webapp/target/*.war /opt/newWars/"
        sshPut remote: remote, from: '/opt/newWars/webapp.war', into: '/opt/docker/', flatten: true
        //sshPut remote: remote, from: "${workspace}/webapp/target/webapp.war", into: '/opt/docker/'
        //sshCommand remote: remote, command: 'cd /opt/docker; docker stop regappv1; docker rm -f regappv1; docker rmi regapp:v1; docker build -t regapp:v1 .; docker rm -f regappv1; docker run -d --name regappv1 -p 8086:8080 regapp:v1;'
        sshCommand remote: remote, command: 'ansible-playbook /opt/docker/create_image_regapp.yml'
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

   stage('CD Stage'){

      withCredentials([usernamePassword(credentialsId: 'b796eb85-7139-459c-bd32-6db10d192339', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]){
        def workspace = env.WORKSPACE
        remote.user = USERNAME
        remote.password = PASSWORD
        echo USERNAME
        echo workspace
        sshCommand remote: remote, command: 'ansible-playbook /opt/docker/kube_deploy.yml'
   }

   
}
}