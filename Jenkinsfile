def jkAgent = 'jk.ntt'
def main_dir = "app-parent"
def jenkinsProps = []
if ("${env.JK_AGENT}" != 'null') {
    jkAgent = "${env.JK_AGENT}"
}
def deployNamespace = ''
def ansibleName = ''
def ansibleGitBranch = ''

node("${jkAgent}") {
    
    /* Define Stage as Purposes */
    stage('SCM Checkout') {
        cleanWs()

        /** Checkout SCM if use pipeline from SCM */
        dir("${main_dir}") {
            def gitRepo = checkout scm
            env.GIT_URL = gitRepo.GIT_URL

            /* Read properties */
            jenkinsProps = readProperties  file: "./Jenkins.properties"
            env.GIT_BRANCH_NAME = gitRepo.GIT_BRANCH.replaceAll( 'origin/', '' )
            env.BRANCH_NAME = "${env.GIT_BRANCH_NAME}".replaceAll( '/', '-' )
            env.GIT_HASH = sh(script: "printf \$(git log -1 --oneline | cut -c 1-7 )", returnStdout: true)
            env.IMAGE_NAME = "${env.QUAY_REGISTRY}/projects/${jenkinsProps.PROJECT_NAME}/${jenkinsProps.APP_NAME}"
            env.IMAGE_VERSION = "${env.BRANCH_NAME}"
            env.IMAGE_TAG_NAME = "${env.IMAGE_NAME}:${env.IMAGE_VERSION}"
            env.APP_PATH = "${jenkinsProps.PROJECT_NAME}/${jenkinsProps.APP_NAME}"
            if ("${env.BRANCH_NAME}" == "testing" && "${env.DEPLOY_ENV}" == 'null') {
               env.DEPLOY_ENV = "test"
            }
            else if ("${env.DEPLOY_ENV}" == 'null') {
                env.DEPLOY_ENV = "dev"
            }
            env.DEPLOY_APP_NAME = "${jenkinsProps.APP_NAME}-${env.DEPLOY_ENV}"
            
            deployNamespace = "${jenkinsProps.DEPLOY_NAMESPACE}"
            if ("${env.DEPLOY_NAMESPACE}" != 'null') {
                deployNamespace = "${env.DEPLOY_NAMESPACE}"
            }

            ansibleName = "${jenkinsProps.ANSIBLE_NAME}"
            if ("${env.ANSIBLE_NAME}" != 'null') {
                ansibleName = "${env.ANSIBLE_NAME}"
            }
            ansibleGitBranch = "${jenkinsProps.ANSIBLE_DEPLOY_K8S_GIT_BRANCH}"
            if ("${env.ANSIBLE_DEPLOY_K8S_GIT_BRANCH}" != 'null') {
                ansibleGitBranch = "${env.ANSIBLE_DEPLOY_K8S_GIT_BRANCH}"
            }
        }

        /* Get source code from a Git repository */
        dir("ansible-deploy") {
            git branch: "${ansibleGitBranch}", credentialsId: "${env.ANSIBLE_DEPLOY_K8S_GIT_CREDENTIALS_ID}", url: "${env.ANSIBLE_DEPLOY_FRONTEND_K8S_GIT_URL}"
        }
    }

    stage('Build Image') {
         /* Run the build image*/
       
        docker.withRegistry("${env.QUAY_REGISTRY_URL}", "${env.QUAY_CREDENTIALS_ID}") {
            def build = docker.build("${env.IMAGE_TAG_NAME}","--build-arg APP_PARENT_DIR=${main_dir} -f ${main_dir}/Dockerfile ./") 

            /* Push the image to the registry */
            build.push()
        }

        /* Remove local image after build */
        sh "docker rmi ${env.IMAGE_TAG_NAME}"
    }

}
