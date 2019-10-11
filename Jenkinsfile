node {

    checkout scm

    env.DOCKER_API_VERSION="1.23"
    
    sh "git rev-parse --short HEAD > commit-id"

    tag = readFile('commit-id').replace("\n", "").replace("\r", "")
    appName = "hello-kenzan"
    imageName = "mgsgomu/${appName}:${tag}"
    env.BUILDIMG=imageName

    stage "Build"
    
        sh "docker build -t ${imageName} -f applications/hello-kenzan/Dockerfile applications/hello-kenzan"
    
    stage "Push"

        sh "docker push ${imageName}" , registrycredentials: 'dockercredentials'

    stage "Deploy"

        kubernetesDeploy configs: "applications/${appName}/k8s/today.yaml", kubeconfigId: 'kenzan_kubeconfig'

}
