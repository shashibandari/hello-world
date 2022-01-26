node {
    def mvnHome
     def remote = [:]
     stage('Checkout') { 
     git branch: 'main', changelog: false, poll: false, url: 'https://github.com/shashibandari/hello-world.git'
        mvnHome = '/opt/maven'
    }
    
    stage('Maven build') {
          // Run the maven build
        withEnv(["MVN_HOME=$mvnHome"]) {
            if (isUnix()) {
                sh '"$MVN_HOME/bin/mvn" -Dmaven.test.failure.ignore clean install package'
            } else {
                bat(/"%MVN_HOME%\bin\mvn" -Dmaven.test.failure.ignore clean package/)
            }
        }
    }
    

  /*  stage('Deploy') {
        sshagent(['dockerkey']) {
           sh "scp -o StrictHostKeyChecking=no webapp/target/*.war dockeradmin@3.131.169.253:/home/dockeradmin/"
        }
    }
  */
    
   /* stage ('Build Image and Run') {
          withCredentials([usernamePassword(credentialsId: 'docker', passwordVariable: 'dockerPassword', usernameVariable: 'dockerUser')]) {
      //    withCredentials([sshUserPrivateKey(credentialsId: 'dockerkey', keyFileVariable: 'identity', passphraseVariable: '', usernameVariable: 'userName')]) {
        remote.name = "node-1"
        remote.host = "3.131.169.253"
        remote.allowAnyHosts = true          
            remote.user = dockerUser
            remote.password = dockerPassword
          //  sshCommand remote: remote, command: "ls -lrt"
            sshCommand remote: remote, command: "docker build -t devops-project ."
             sshCommand remote: remote, command: "docker images"
              sshCommand remote: remote, command: "docker run -d --name devops-container -p 8080:8080 devops-project"
        //  sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
          //sh 'docker push devopsfaqs/devops-project'
        }
    }
    */
    stage('Docker Build') {
        sh 'docker build -t devopsfaqs/devops-project .'
    }
    
    stage('Push to Dockerhub'){
        withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
          sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
          sh 'docker push devopsfaqs/devops-project'
        }
        
    }
}
