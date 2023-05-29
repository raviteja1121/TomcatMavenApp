pipeline {
    agent any
    tools {
        maven 'mvn'
    }
    stages {
        stage('clone') {
            steps {
                git 'https://github.com/raviteja1121/TomcatMavenApp.git'
            }
        }
        stage('build') {
            steps {
            when {
             tag 'release-3.0'
             }
            sh 'mvn package'
        }
        }
        stage('push_to_s3') {
            steps {
               withCredentials([[
    $class: 'AmazonWebServicesCredentialsBinding',
    credentialsId: "6ee9bd71-afe6-4b92-abc5-0b5cd8c8e5ec",
    accessKeyVariable: 'AWS_ACCESS_KEY_ID',
    secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
]]) {
    sh "aws s3 ls"
    sh "aws s3 cp $WORKSPACE/target/ s3://mavenbuckettej --recursive --exclude '*' --include '*.war'"
}
            }   
        }
    }
    post {
        always {
                cleanWs cleanWhenAborted: false, cleanWhenFailure: false, cleanWhenNotBuilt: false, cleanWhenUnstable: false, notFailBuild: true
        }
    }
}
