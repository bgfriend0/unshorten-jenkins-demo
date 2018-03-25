node('docker)' {

    checkout scm

    env.DOCKER_API_VERSION="1.9.1"
    
    sh "git rev-parse --short HEAD > commit-id"

    tag = readFile('commit-id').replace("\n", "").replace("\r", "")
    appName = "link-unshorten"
    registryHost = "127.0.0.1:30400/"
    imageName = "${registryHost}${appName}:${tag}"
    env.BUILDIMG=imageName

    stage "Build"
        sh "echo building docker image..."
        sh "docker build -t ${imageName} . "

    stage "Push"
        sh "Pushing image to local repo..."
        sh "docker push ${imageName}"
        sh "docker images"

    stage "Scan Docker Image"
        sh "echo scanning docker image for vulnerabilities..."

    stage "Source Code Static Analysis"
        sh "echo Running static analysis scan..."

    stage "Kubernetes Analysis"
        sh "Scanning Kubernetes configs for vulnerabilities"

    stage "Deploy"

        sh "sed 's#127.0.0.1:30400/link-unshorten:latest#'$BUILDIMG'#' k8s/deployment.yaml | kubectl apply -f -"
        sh "kubectl rollout status deployment/link-unshorten"
}