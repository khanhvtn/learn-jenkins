def main_dir = "app-parent"
node {

    /* Define Stage as Purposes */
    stage('SCM Checkout') {
        agent { docker { image 'node:16.17.1-alpine' } }
        cleanWs()

        /** Checkout SCM if use pipeline from SCM */
        dir("${main_dir}") {
            def gitRepo = checkout scm
            env.GIT_URL = gitRepo.GIT_URL

            sh 'node -v'

        }
    }
}


