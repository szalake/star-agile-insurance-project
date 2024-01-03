node{
    
    def mavenHome
    def mavenCMD
    def docker
    def dockerCMD
    def tagName
    def containerName="insuranceproj"
    def tag="latest"
    def dockerHubUser="sharmilagaikwad29"
    def password="Aadi123tva$"

    
    stage('prepare enviroment'){
        echo 'initialize all the variables'
        mavenHome = tool name: 'maven' , type: 'maven'
        mavenCMD = "${mavenHome}/bin/mvn"
        docker = tool name: 'docker' , type: 'org.jenkinsci.plugins.docker.commons.tools.DockerTool'
      //  dockerhub = tool name: 'dockerhub' , type: 'org.jenkinsci.plugins.plaincredentials.StringCredentials'
        dockerCMD = "${docker}/bin/docker"
        tagName="3.0"
    }
    
    stage('git code checkout'){
        try{
            echo 'checkout the code from git repository'
            git 'https://github.com/szalake/star-agile-insurance-project.git'
        }
        catch(Exception e){
            echo 'Exception occured in Git Code Checkout Stage'
            currentBuild.result = "FAILURE"
            emailext body: '''Dear All,
            The Jenkins job ${JOB_NAME} has been failed. Request you to please have a look at it immediately by clicking on the below link. 
            ${BUILD_URL}''', subject: 'Job ${JOB_NAME} ${BUILD_NUMBER} is failed', to: 'sharmilagaikwad29@gmail.com'
        }
    }
    
    stage('Build the Application'){
        echo "Cleaning... Compiling...Testing... Packaging..."
        //sh 'mvn clean package'
        sh "${mavenCMD} clean package"        
    }
    
   /* stage('publish test reports'){
        publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '/var/lib/jenkins/workspace/Capstone-Project-Live-Demo/target/surefire-reports', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: '', useWrapperFileDirectly: true])
    }*/
    
    stage('Containerize the application'){
        echo 'Creating Docker image'
         sh "docker build -t $containerName:$tag --pull --no-cache ."
       // sh "${dockerCMD} build -t sharmilagaikwad29/insuranceproj:${tagName} ."
    }
  /* stage('Push to Docker Registry'){
        withCredentials([usernamePassword(credentialsId: 'dockerhubtoken', usernameVariable: 'dockerUser', passwordVariable: 'dockerPassword')]) {
            sh "docker login -u $dockerUser -p $dockerPassword"
            sh "docker tag $containerName:$tag $dockerUser/$containerName:$tag"
            sh "docker push $dockerUser/$containerName:$tag"
            echo "Image push complete"
        }
    }*/

    
    
    stage('Pushing it ot the DockerHub'){
        echo 'Pushing the docker image to DockerHub'
        withCredentials([string(credentialsId: 'dockerhubtoken', variable: 'Password')]) {
        sh "${dockerCMD} login -u sharmilagaikwad29 -p ${Password}"
        sh "${dockerCMD} push sharmilagaikwad29/insuranceproj:${tagName}"
            
        }
        
  /*  stage('Configure and Deploy to the test-server'){
        ansiblePlaybook become: true, credentialsId: 'ansible-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'ansible-playbook.yml'
    }*/
        
        
    }
}



