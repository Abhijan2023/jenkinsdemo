pipeline {
  agent any
  stages {
    stage ('Build') {
      steps {
        sh 'printenv'
        sh 'docker build -t frankisinfotech/jenkinsdemo:""$GIT_COMMIT"" .'
      }
    }
    
     stage ('Publish to DockerHub') {
      steps {
        withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
          sh 'docker push frankisinfotech/jenkinsdemo:""$GIT_COMMIT""'
         }
       }
     }
    stage ('Publish to ECR') {
      steps {
        sh 'aws ecr-public get-login-password --region eu-west-2 | docker login --username AWS --password-stdin public.ecr.aws/t7e2c6o4'
        sh 'docker build -t frankdemo .'
        sh 'docker tag frankdemo:latest public.ecr.aws/t7e2c6o4/frankdemo:latest'
        sh 'docker push public.ecr.aws/t7e2c6o4/frankdemo:latest'
         }
       }
     }
  }
}
