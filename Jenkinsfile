node {
    def app

    stage('Clone repository') {
        /* Clone repo to workspace*/

        checkout scm
    }

    stage('Build image') {
        /* This makes the image
         * the left side of the '/' is the username, the right is the repo name*/

        app = docker.build("dockerkieran/buildserver-image")
    }

    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         * For this example, we're using a Volkswagen-type approach ;-) */

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
}
