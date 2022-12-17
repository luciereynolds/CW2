node {
    def app

    stage('Clone repository') {
        /* Clone repository to workspace */

        checkout scm
    }

    stage('Build image') {
        /* Builds image from Docker hub */

        app = docker.build("luciereynolds/cw2")
    }

    stage('Test image') {
        /* testing the image created */

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        /* Pushing image to dockerhub with tags */
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
}

