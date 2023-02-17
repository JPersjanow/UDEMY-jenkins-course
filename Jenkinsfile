node {
   def commit_id
   stage('Preparation') {
     checkout([$class: 'GitSCM',
          branches: [[name: '*/master']],
          userRemoteConfigs: [[url: 'https://github.com/wardviaene/docker-demo.git']]])
     sh "git rev-parse --short HEAD > .git/commit-id"                        
     commit_id = readFile('.git/commit-id').trim()
   }
   stage('test') {
     nodejs(nodeJSInstallationName: 'nodejs') {
       sh 'npm install --only=dev'
       sh 'npm test'
     }
   }
   stage('docker build/push') {
     docker.withRegistry('https://index.docker.io/v2/', 'cb190741-e666-40b6-92ff-0998a2ff008c') {
       def app = docker.build("persildocker/udemy-jenkins${commit_id}", '.').push()
     }
   }
}