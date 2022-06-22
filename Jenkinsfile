node('web-server') {

stage("Git Clone"){
    sh 'rm -rf *'
    sh 'git clone https://github.com/midhunkangumara/Angular.git'
     
}

stage("Docker build"){
    sh 'mv Angular/* .'
    sh 'docker version'
    sh 'docker build -t angular-web .'  
    sh 'docker image list'
    sh 'docker tag angular-web kmmidhun/angular-web:1'
  }

stage("Docker Login"){
    
        withCredentials([string(credentialsId: 'DOCKER_HUB_PASSWORD', variable: 'PASSWORD')]) {
            sh 'docker login -u kmmidhun -p $PASSWORD'
        }
    } 
stage("Push Image to Docker Hub"){
        sh 'docker push  kmmidhun/angular-web:1'
    }
 
stage("SSH to jiso server") {
        def remote = [:]
        remote.name = 'node2'
        remote.host = '10.0.0.151'
        remote.user = 'jiso'
        remote.password = 'jiso'
        remote.allowAnyHosts = true
        stage("pull image from repo"){
               sshCommand remote: remote, command: "pwd"
               sshCommand remote: remote, command: "hostnamectl"
               sshCommand remote: remote, command: "docker version"
               sshCommand remote: remote, command:  "docker pull kmmidhun/angular-web:1"
               }

        stage('running container'){
             sshCommand remote: remote, command: "docker run -d --rm --name myweb -p 80:80 kmmidhun/angular-web:1 "
             }
    }
}
