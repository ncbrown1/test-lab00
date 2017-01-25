node {
  stage('Build') {
    checkout scm
    sh 'ls'
    sh 'g++ -o hello hello.cpp'
    archiveArtifacts artifacts: 'hello', fingerprint: true
  }
  stage('Run') {
    sh './hello > hello_output.out'
    sh 'cat hello_output.out'
    archiveArtifacts artifacts: 'hello_output.out', fingerprint: true
  }
}
