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

    stage('Deploy') {
        /* Deploy passed builds to Kubernetes as a rolling update */
        sshagent(['my-ssh-key']) {
	        sh 'ssh ubuntu@3.87.194.59 kubectl set image deployments/cw02 cw02-6475795f-fv94x=luciereynolds/cw02:1.0'
        }
    }

}

