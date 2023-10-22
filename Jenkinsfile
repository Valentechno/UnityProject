node
{
def mavenHome = tool name: "maven3.9.5"

    stage('Maven Build')
    {
        sh "${mavenHome}/bin/mvn clean package"
    }
    
    stage('SonarQube Code Quality report')
        {
         sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    stage('Deploy Artifacts to Nexus')
     {
         sh "${mavenHome}/bin/mvn deploy"
    }
    
    stage('Build Docker Image')
    {
        
        sh "docker build -t valentechno/project1 ."

    }
    
    stage('Push Docker Image to Docker Hub')
    {
       withCredentials([string(credentialsId: 'DockerHubCredentials', variable: 'DockerHubCredentials')]) {
       sh " docker login -u valentechno -p ${DockerHubCredentials} "
     }

        sh " docker push valentechno/project1 "
    }
    
    stage('Deploy to EKS Kubernetes Cluster')
    {
  
        kubernetesDeploy(
            configs: 'springboot-app-deployment.yml' ,
            kubeconfigId: 'KubernetesClusterConfig' ,
            enableConfigSubstitution: true
            )
        
       // sh 'kubectl apply -f springboot-app-deployment.yml'
    }

    stage('EmailNotification')
 {
 mail bcc: 'abihngeng@yahoo.com', body: '''Build is over
 Thanks,
 Mithun Technologies,
 9980923226.''', cc: 'abihngeng@yahoo.com', from: '', replyTo: '', subject: 'Build is over!!', to: 'abihngeng@gmail.com'
 }
 
 }
