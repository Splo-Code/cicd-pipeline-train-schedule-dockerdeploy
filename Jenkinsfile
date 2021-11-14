pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage {
            when {
             branch 'master'   
            }
            step {
                script {
                app = docker.build("zakh99/train-schedule2")
                    app.inside {
                     sh 'echo $(curl localhost:8080)'   
                    }
                }
            }
        }
        stage {
            when {
             branch 'master'   
            }
            step {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
    }
}
