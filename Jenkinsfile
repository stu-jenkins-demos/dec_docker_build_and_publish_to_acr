pipeline {
      agent {
        kubernetes {
          label 'dec_docker_build_and_publish'
          defaultContainer 'jnlp'
          yaml """
    apiVersion: v1
    kind: Pod
    metadata:
      labels:
        some-label: some-label-value
    spec:
      containers:
      - name: awscli
        image: ${awscliContainer}
        command:
        - cat
        tty: true
      - name: docker
        image: ${dockerContainer}
        command: ["cat"]
        tty: true
        volumeMounts:
        - mountPath: /var/run/docker.sock
          name: docker-socket
      volumes:
      - name: docker-socket
        hostPath:
          path: /var/run/docker.sock
          type: Socket
    """
        }
      }
      stages {
          stage('Build and push Image') {
              steps {
                  container('docker'){
                      sh "docker login --username $application_id --password $key ntweekly.azurecr.io"
                      sh "docker build ."
                      //sh "docker push ${ecrRepo}:${ecrTag}"
                  }
              }
          }
      }
    }
