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
      - name: docker
        image: docker:18.06
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
      environment { 
        ecrRepo = 'stusreg.azurecr.io/dec_docker_build_and_publish_to_acr:latest'
      }
      stages {
          stage('Build and push Image') {
              steps {
                  container('docker'){
                    withCredentials([usernamePassword(credentialsId: 'acr_login', passwordVariable: 'key', usernameVariable: 'application_id')]) {
                      //ecrRepo = 'stusreg.azurecr.io/dec_docker_build_and_publish_to_acr:latest'
                      sh "docker login --username $application_id --password $key stusreg.azurecr.io"
                      sh "docker build ."
                      //sh "docker push ${ecrRepo}:${ecrTag}"
                    }
                  }
              }
          }
      }
    }
