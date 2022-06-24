
 
pipeline {
    /* specify nodes for executing */
  agent any
    stages {
        /* checkout repo */
        stage('Checkout SCM') {
            steps {
                checkout([
                 $class: 'GitSCM',
                 branches: [[name: 'main']],
                 userRemoteConfigs: [[
                    url: 'https://github.com/midhunkangumara/Angular.git',
                    credentialsId: 'GITHUB',
                 ]]
                ])
            }
        }
         stage("Docker build"){
    //sh 'mv Angular/* .'
    steps {  sh 'docker version'
        sh 'docker build -t angular-web .'
        sh 'docker image list'
        sh 'docker tag angular-web kmmidhun/angular-web:1'
    }
  }
stage("Docker Login"){
       steps { withCredentials([string(credentialsId: 'DOCKER_HUB_PASSWORD', variable: 'PASSWORD')]) {
            sh 'docker login -u kmmidhun -p $PASSWORD'
       }
        }
    }
stage("Push Image to Docker Hub"){
      steps { sh 'docker push  kmmidhun/angular-web:1'
      }
    }

 stage("pull image from repo"){
        steps { sh 'pwd'
               sh 'hostnamectl'
               sh 'docker version'
               sh 'docker pull kmmidhun/angular-web:1'
        }
    }
 stage('running container'){
           steps {  sh 'docker run -d --rm --name myweb -p 80:80 kmmidhun/angular-web:1'
             }
      }
    }
 
    /* Cleanup workspace */
    post {
       always {
           deleteDir()
       }
   }
}

