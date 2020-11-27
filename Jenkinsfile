pipeline {
  agent any
  stages {
    stage('docker-build') {
      steps {
        sh 'docker build -t redis:v1 .'
      }
    }

    stage('docker-run') {
      steps {
        sh '''docker_redis=`docker ps -a | grep redisv1 | awk \'{print $1}\'`
if [ -z $docker_redis ];then
  docker run -it -d --rm --name redisv1 redis:v1
else
  docker stop redisv1
fi'''
      }
    }

    stage('Dockerfile') {
      parallel {
        stage('Dockerfile') {
          steps {
            sh '''cat <<EOF>> ./redis/Dockerfile
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

        stage('docker-cp') {
          steps {
            sh 'docker cp redisv1:/rootfs.tar .'
          }
        }

      }
    }

    stage('build-image') {
      steps {
        sh '''docker build -t redis:v2 -f ./redis/Dockerfile .
docker images'''
      }
    }

  }
}