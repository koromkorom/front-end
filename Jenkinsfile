pipeline {
  agent {
    kubernetes {
      yaml """\
        apiVersion: v1
        kind: Pod
        metadata:
          labels:
            some-label: some-label-value
        spec:
          containers:
          - name: node
            image: node:latest
            command:
            - cat
            tty: true
        """.stripIndent()
    }
  }
  stages {
    stage('Pre install') {
      steps {
        container('node') {
          sh 'sudo apt-get install -y make'
          sh 'make test-image deps'
        }
      }
    }
    stage('Run npm install') {
      steps {
        container('node') {
          sh 'npm install'
        }
      }
    }
    stage('Make test') {
      steps {
        container('node') {
          sh 'make test'
        }
      }
    }
  }
}
