def main_dir = "app-parent"

pipeline{
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

