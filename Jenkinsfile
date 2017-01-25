node {
  stage('Build') {
    checkout scm
    sh 'g++ -o hello hello.cpp'
    stash includes: 'hello', name: 'hello'
  }
  stage('Run') {
    unstash 'hello'
    sh './hello > hello_output.out'
    sh 'cat hello_output.out'
    archiveArtifacts artifacts: 'hello_output.out', fingerprint: true
  }
}
