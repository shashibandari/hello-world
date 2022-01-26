node {
    def mvnHome
       
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
    

    stage('Deploy') {
            sshagent(['dockerkey']) {
       // sh 'ssh dockeradmin@3.131.169.253 rm -rf webapp.war'
        sh "scp -o StrictHostKeyChecking=no webapp/target/*.war dockeradmin@3.131.169.253:/home/dockeradmin/"
        
    }
    }
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
