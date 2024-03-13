pipeline {
    agent any

    stages {
        stage("Clone project") {
            steps {
                git branch: 'main', url: 'https://github.com/jed212/Jenkins-ci-cd.git'
            }
        }

        stage("Build project with test execution") {
            steps {
                sh "./gradlew build"
            }
        }

        stage("Deploy to DockerHub with Jib") {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'DOCKER_CRED') {
                        sh "echo ${DOCKER_PASSWORD} | docker login -u ${DOCKER_USERNAME} --password-stdin"
                        sh "./gradlew jib -Djib.to.auth.username=${DOCKER_USERNAME} -Djib.to.auth.password=${DOCKER_PASSWORD} -Djib.to.image=jedfavour/jenkins-docker-project:latest --stacktrace"
                    }
                }
            }
        }

        stage("Generate Jacoco Report") {
            steps {
                jacoco(
                    execPattern: '**/*.exec',
                    sourcePattern: 'src/main/java',
                    exclusionPattern: 'src/test*'
                )
            }
        }
    }

    post {
        always {
            junit '**/build/test-results/test/*.xml'
            jacoco(execPattern: '**/*.exec')
        }
    }
}