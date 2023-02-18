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

            sh 'ls'

        }
    }

}
