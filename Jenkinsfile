properties([pipelineTriggers([githubPush()])])

node('linux') {
    stage('Unit Tests') {
      git 'https://github.com/Sharifah11/java-project.git'
      sh 'ant -f test.xml -v'
      junit 'reports/result.xml'
    }
    stage('Build') {
      sh 'ant -f build.xml -v'
    }
    stage('Deploy'){
        sh 'aws s3 cp /workspace/java-pipeline/dist/rectangle-${BUILD_NUMBER}.jar s3://sharifah11-assignment-4'
    }
    stage('Report'){
        withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'd02b01fc-5257-40e8-8dc1-947d87e05a41', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
        sh 'aws cloudformation describe-stack-resources --stack-name jenkins --region us-east-1' 
       }
    }
}
