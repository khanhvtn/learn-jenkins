def main_dir = "app-parent"
node {

     agent { docker { image 'node:16.17.1-alpine' } }

    /* Define Stage as Purposes */
    stage('SCM Checkout') {
        cleanWs()

        /** Checkout SCM if use pipeline from SCM */
        dir("${main_dir}") {
            def gitRepo = checkout scm
            env.GIT_URL = gitRepo.GIT_URL

            sh 'node -v'

        }
    }
}


