pipeline{
    agent any
    tools{
        gradle 'gradle-7.1.1'
    }
    stages{
        stage('git checkout'){
            steps{
                git branch: 'master',
    url: 'https://github.com/cloudtechmasters/spring-boot-gradle-demo.git'
            }
        }
        stage('build gradle project'){
            steps{
                sh 'gradle clean build'
            }
        }
        
        stage('build docker image from file'){
            steps{
                sh 'docker image build -t 164867375549.dkr.ecr.us-east-1.amazonaws.com/spring-boot-gradle-demo:${BUILD_NUMBER} .'
            }
        }
        
        stage("Push the docker image to ecr registry"){
            steps{
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aws-ecr-credentials', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                    sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 164867375549.dkr.ecr.us-east-1.amazonaws.com'
                    sh 'docker image push 164867375549.dkr.ecr.us-east-1.amazonaws.com/spring-boot-gradle-demo:${BUILD_NUMBER}'
                }
            }
        }
        
    }
}
