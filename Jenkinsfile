node {
  stage("Clone project") {
    git branch: 'main', url: 'https://github.com/jed212/Jenkins-ci-cd.git'
  }

  stage("Build project with test execution") {
    sh "./gradlew build"
  }
  
  stage("Deploy to DockerHub with Jib") {
    withCredentials([usernamePassword(credentialsId: 'DOCKER_CRED', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
        sh '''
        echo "${PASSWORD}" | docker login -u "${USERNAME}" --password-stdin
        ./gradlew jib -Djib.to.auth.username="${USERNAME}" -Djib.to.auth.password="${PASSWORD}"
        '''
    }
  }

  jacoco(
    execPattern: '**/*.exec',
    sourcePattern: 'src/main/java',
    exclusionPattern: 'src/test*'
  )
}
