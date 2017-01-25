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
    stash includes: 'hello_output.out', name: 'hello_output.out'
    archiveArtifacts artifacts: 'hello_output.out', fingerprint: true
  }
  stage('Diff') {
    unstash 'hello_output.out'
    sh 'mv hello_output.out hello_output_local.out'
    sh 'ls'
    step ([$class: 'CopyArtifact',
          projectName: 'ncbrown1/solution-lab00/master',
          filter: 'hello_output.out']); 
    sh 'ls'
    def ret = sh returnStatus: true, script: 'diff hello_output_local.out hello_output.out > hello.diff'
    sh 'cat hello.diff'
    archiveArtifacts artifacts: 'hello.diff', fingerprint: true
    if (ret != 0) { sh 'fail' }
  }
}
