node {

   stage 'Checkout'
   git 'https://github.com/MitchyBAwesome/dogs.git'

   stage 'Build Dockerfile'
   docker.build('dogs')

   stage 'Build AWSCLI Container'
   sh 'docker build -f ./buildresources/AWSCLI -t awscli .'

   stage 'Build Klar Container'
   sh 'docker build -f ./buildresources/klar/Dockerfile -t klar:latest .'

   stage 'Push to ECR'
   //sh "docker run -v \${HOME_PATH}/.aws:/root/.aws awscli "
   sh ("eval \$(docker run -v \${HOME_PATH}/.aws:/root/.aws awscli aws ecr get-login --region ${REGION} --no-include-email | sed 's|https://||')")
   docker.withRegistry('https://${ECR_REPO}') {
       docker.image('dogs').push('${BUILD_NUMBER}')
   }

   stage 'Scan Pushed Image for Vulnerabilites'
   docker.image('klar:latest').inside {
     stage("Testing Pushed Image") {
       sh "/bin/sh -c /klar \${ECR_REPO}:\${BUILD_NUMBER}"
     }
   }
   /** sh DOCKER_USER=AWS DOCKER_PASSWORD=${PASSWORD} ./klar 710487389845.dkr.ecr.us-east-1.amazonaws.com/dogs:26

/**   stage 'update application'

   parallel(
        ecs: {node {
        docker.image('awscli').inside{
            git 'https://github.com/omarlari/aws-container-sample-app.git'
            sh 'sed -i s/REPO/${ECR_REPO}/g task-definition-hello.json'
            sh 'sed -i s/BUILD/${BUILD_NUMBER}/g task-definition-hello.json'
            sh 'aws ecs register-task-definition --cli-input-json file://task-definition-hello.json --family ${APP} --region ${REGION}'
        }
        }},
        kubernetes: { node {
        docker.image('kubectl').inside("--volume=/home/ec2-user/.kube:/config/.kube"){
            sh 'kubectl describe deployment ${APP}'
            sh 'kubectl set image deployment/${APP} movies=${ECR_REPO}/hello:${BUILD_NUMBER}'
            sh 'kubectl describe deployment ${APP}'
            }
        }},
        swarm: { node {
            sh "echo deploying to swarm"
        }}
   )


   stage 'update ECS service'
   docker.image('awscli').inside{
       sh 'aws ecs update-service --cluster ${ECS_CLUSTER} --service ${APP} --task-definition ${APP} --region ${REGION}'
   }
**/
}
