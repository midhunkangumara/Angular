

node{
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

 stage("pull image from repo"){
               sh 'pwd'
               sh 'hostnamectl'
               sh 'docker version'
               sh 'docker pull kmmidhun/angular-web:1'
               }
 stage('running container'){
             sh 'docker run -d --rm --name myweb -p 80:80 kmmidhun/angular-web:1'
             }
    }


def attachments = [
  [
    text: 'I find your lack of faith disturbing!',
    fallback: 'Hey, Vader seems to be mad at you.',
    color: '#ff0000'
  ]
]

slackSend(channel: "Midhun K M", attachments: attachments)