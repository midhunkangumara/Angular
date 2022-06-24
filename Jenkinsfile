
node {
     try {
    notifyStarted()
stage('cloning'){
      checkout([
                 $class: 'GitSCM',
                 branches: [[name: 'main']],
                 userRemoteConfigs: [[
                    url: 'https://github.com/midhunkangumara/Angular.git',
                    
                 ]]
                ])
    }

stage("Docker build"){
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

 stage("pull image from repo"){
               sh 'pwd'
               sh 'hostnamectl'
               sh 'docker version'
               sh 'docker pull kmmidhun/angular-web:1'
               }
 stage('running container'){
             sh 'docker run -d --rm --name myweb -p 80:80 kmmidhun/angular-web:1'
             }
               notifySuccessful()
  } catch (e) {
    currentBuild.result = "FAILED"
    notifyFailed()
    throw e
  }
}    
def notifyStarted() {
 
  slackSend (color: '#FFFF00', message: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")

}
def notifySuccessful() {
  slackSend (color: '#00FF00', message: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
}

def notifyFailed() {
  slackSend (color: '#FF0000', message: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")

}


