node {
  stage("Clone project") {
    git branch: 'main', url: 'https://github.com/jed212/Jenkins-ci-cd.git'
  }

  stage("Build project with test execution") {
    sh "./gradlew build"
  }
  jacoco(
    execPattern: '**/*.exec',
    sourcePattern: 'src/main/java',
    exclusionPattern: 'src/test*'
  )
}
