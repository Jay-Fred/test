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
        sh '''container=`docker ps -a -q`
if [ $?==0 ];then
  docker run -it -d --rm --name redisv1 redis:v1
else
  for i in $container
    do
      docker stop $i
      docker rm $i
    done
fi
'''
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
        sh '''docker build -t redis:v2 -f ./redis/Dockerfile
docker images'''
      }
    }

  }
}