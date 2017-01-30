@Library('anacapa-jenkins-lib') import static edu.ucsb.cs.anacapa.pipeline.Lib.*

node {
  stage('Build') {
    checkout scm
    sh 'g++ -o hello hello.cpp'
    stash includes: 'hello', name: 'hello'
  }
  stage('CheckJSON') {
    def obj = parseJSON(readFile("testables.json"))
    println obj
  }
  
  for (int i=0; i< 2; ++i) {  
    stage ("Stage #${i}") {
      print 'Hello, world $i!'
    }
  }

  stage ("Stage Parallel") {
    def branches = [:]
    for (int i = 0; i < 10; i++) {
      branches["split${i}"] = {
        stage ("Stage parallel- #"+i) {
         echo  'Starting sleep'
         sleep 4
         echo  'Finished sleep'
        }
      }
    }

    parallel branches
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
