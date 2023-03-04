pipeline{
    agent any
    tools{
        maven 'mymaven'
    }
    stages{
    stage('checkout'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'git', url: 'https://github.com/ariffhere/java-maven-war-app.git']])
            }
             
        }
        
        stage('build'){
            steps{
              sh 'mvn clean install'
            }
        }
        stage("sonar"){
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'mysonar') {
                        sh "${tool("mysonar")}/bin/sonar-scanner \
                        -Dsonar.projectKey=java-maven-war-app \
                        -Dsonar.sources=. \
                        -Dsonar.java.binaries=target \
                        -Dsonar.host.url=http://172.31.5.133:9000 \
                        -Dsonar.login=sqp_37bb123b8b54d4e281b8fce80dca1efc1c3cec76"
    
                    }
                }
            }

        }
        stage("artifact-upload"){
            steps{
                sh 'mvn -s settings.xml deploy'
            }
        }
    }
}
