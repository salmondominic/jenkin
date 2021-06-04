node {
    stage ("Pull source code from GitHub"){
        git 'https://github.com/salmondominic/docker.git'
    }
    
    stage ("Build docker images"){
        sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
        sh 'docker image tag $JOB_NAME:v1.$BUILD_ID salmondevops/$JOB_NAME:v1.$BUILD_ID'
        sh 'docker image tag $JOB_NAME:v1.$BUILD_ID salmondevops/$JOB_NAME:latest'
    }
    
    stage ("Push to DockerHub"){
        withCredentials([string(credentialsId: 'dockerhubpass', variable: 'dockerhubpass')]) {
    // some block
    sh 'docker login -u salmondevops -p ${dockerhubpass}'
    sh 'docker image push salmondevops/$JOB_NAME:v1.$BUILD_ID'
    sh 'docker image push salmondevops/$JOB_NAME:latest'
    sh 'docker image rmi $JOB_NAME:v1.$BUILD_ID salmondevops/$JOB_NAME:v1.$BUILD_ID salmondevops/$JOB_NAME:latest'
}
    }
    
    stage ("Deploy a container into Ec2 instance") {
        def docker_run = 'docker run -it -d -p 9090:80 --name apache-web3 salmondevops/pipeline-script:latest bash'
        sshagent(['dockerhostpass']) {
    // some block
    sh "ssh -o StrictHostKeyChecking=no ec2-user@13.233.141.74 ${docker_run}"
}
    }
}

