def REPO = "https://github.com/hbbpb/demo-worker"
def NAME = "worker"
def HARBOR_URL = "10.202.129.147"
def HARBOR_PROJECT = "library"
def IMAGE = "${HARBOR_URL}/${HARBOR_PROJECT}/${NAME}:${BUILD_NUMBER}"
def DEPLOY_REPO = "https://github.com/hbbpb/demo-deploy"

node {
    stage("checkout"){
        git "${REPO}"
    }

    stage("build"){
        sh "docker build -t ${IMAGE} ."

        withCredentials([[$class: "UsernamePasswordMultiBinding", credentialsId: "harbor", usernameVariable: "USERNAME", passwordVariable: "PASSWORD"]]) {
            sh "docker login --username=${USERNAME} --password=${PASSWORD} ${HARBOR_URL}"
        }

        sh "docker push ${IMAGE}"
    }

    stage("checkout deploy script"){
        git "${DEPLOY_REPO}"
    }

    stage("deploy"){
        sh "./deploy.sh ${NAME} dev ${IMAGE}"
    }
}
