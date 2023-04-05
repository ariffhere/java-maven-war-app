pipeline{
    agent{
        label 'master'
    }
    tools{
        maven 'mymaven'
    }
    stages{
    stage('checkout'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'git', url: 'https://github.com/ariffhere/java-maven-war-app.git']])
            }
             
        }
        stage('compile'){
            steps{
                sh 'mvn compile'
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
                        -Dsonar.host.url=http://3.235.135.135:9000 \
                        -Dsonar.login=sqp_6b1fa5446007869f30ed443bbf9a7da396acf2b5"
                    }
                }
            }

        }
        stage("artifact-upload"){
            steps{
                sh 'mvn -s settings.xml deploy'
            }
        }
        stage("artifact-deployment"){
            agent{
                label 'ANSIBLE'
            }
            steps{
                sh 'ansible-playbook -i inventory.yml deployment_playbook.yml -e "build_number=${BUILD_NUMBER}\"'
            }
        }
            
    }
}
