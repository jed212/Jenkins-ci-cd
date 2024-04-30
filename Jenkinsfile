node {
  stage("Clone project") {
    git branch: 'main', url: 'https://github.com/jed212/Jenkins-ci-cd.git'
  }

  stage("Build project with test execution") {
    sh "./gradlew build"
  }
  
  stage("Deploy to DockerHub with Jib") {
    parallel([
        docker: {
            withCredentials([usernamePassword(credentialsId: 'DOCKER_CRED', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
              sh '''
              echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
              ./gradlew jib -Ddocker.username=$DOCKER_USERNAME -Ddocker.password=$DOCKER_PASSWORD --stacktrace
              '''
            }
        },
    ])
  }

  jacoco(
    execPattern: '**/*.exec',
    sourcePattern: 'src/main/java',
    exclusionPattern: 'src/test*'
  )
}
