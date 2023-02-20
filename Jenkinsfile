def main_dir = "app-parent"
node {

    /* Define Stage as Purposes */
    stage('SCM Checkout') {
        cleanWs()

        /** Checkout SCM if use pipeline from SCM */
        dir("${main_dir}") {
            def gitRepo = checkout scm
            env.GIT_URL = gitRepo.GIT_URL
            def props = readJSON text: '{ "key": null, "a": "b" }', returnPojo: true
            echo props['version']
        }
    }
}


