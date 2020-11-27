pipeline {
  agent any
  stages {
    stage('docker-build') {
      steps {
        sh 'docker build -t redis:v1 .'
      }
    }

    stage('docker-run') {
      parallel {
        stage('docker-run') {
          steps {
            sh 'docker run -it -d --rm --name redisv1 redis:v1'
          }
        }

        stage('docker-cp') {
          steps {
            sh 'docker cp redisv1:/rootfs.tar .'
          }
        }

      }
    }

    stage('Dockerfile') {
      steps {
        sh '''cat <<EOF>> redisfile
FROM scratch
ADD rootfs.tar /
WORKDIR /data
VOLUME /data
EXPOSE 6379
ENTRYPOINT ["/usr/bin/redis-server"]
CMD ["--port", "6379"]
EOF'''
      }
    }

    stage('build-image') {
      steps {
        sh '''docker build -t redis:v2 -f redisfile
docker images'''
      }
    }

  }
}