


def main_dir = "app-parent"


pipeline {
    agent { docker { image 'node:16.17.1-alpine' } }
    stages {
        stage('Build') {
            steps {
                cleanWs()
                sh "node -v"


                /** Checkout SCM if use pipeline from SCM */
                // dir("${main_dir}") {
                //     def gitRepo = checkout scm
                //     env.GIT_URL = gitRepo.GIT_URL
                //     def props = readJSON file: 'package.json', returnPojo: true
                //     echo props['version']
                // }
            }
        }
    }
}


