pipeline{
    agent any
    tools{
        maven 'mymaven'
    }
    stages{
    stage('checkout'){
            steps{
                checkout scmGit(branches: [[name: '*/ak']], extensions: [], userRemoteConfigs: [[credentialsId: 'git', url: 'https://github.com/ariffhere/java-maven-app.git']])
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
                        -Dsonar.projectKey=java-maven-app \
                        -Dsonar.sources=. \
                        -Dsonar.java.binaries=target \
                        -Dsonar.host.url=http://172.31.5.133:9000 \
                        -Dsonar.login=sqp_bd758495eec154189bd65eb6678641cab007a1bc"
    
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
