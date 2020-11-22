node {
    stages{

    stage('Docker node test') {
      agent {
        docker {
          image 'node:7-alpine'
          args '--name docker-node' // list any args
        }
      }
      steps {
        // Steps run in node:7-alpine docker container on docker slave
        sh 'node --version'
      }
    }

    stage('Docker maven test') {
      agent {
        docker {
          image 'maven:3-alpine'
        }
      }
      }
      steps {
        // Steps run in maven:3-alpine docker container on docker slave
        sh 'mvn --version'
      }
    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */
        echo 'clone stage'
        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */
        echo 'build stage'
        app = docker.build("getintodevops/hellonode")
    }

    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         * For this example, we're using a Volkswagen-type approach ;-) */
        echo 'test stage'
        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        echo 'push stage'
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
}
