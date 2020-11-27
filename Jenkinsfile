pipeline {
  agent any
  stages {
    stage('docker-build-v1') {
      steps {
        sh 'docker build -t redis:v1 .'
      }
    }

    stage('docker-redis-v1') {
      steps {
        sh '''docker_redis=`docker ps -a | grep redisv1 | awk \'{print $1}\'`
if [ -z $docker_redis ];then
  docker run -it -d --rm --name redisv1 redis:v1
fi'''
      }
    }

    stage('docker-cp') {
      steps {
        sh 'docker cp redisv1:/rootfs.tar .'
      }
    }

    stage('dockerfile') {
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

    stage('docker-build-v2') {
      steps {
        sh '''docker build -t redis:v2 -f ./redis/Dockerfile .
docker images'''
      }
    }

    stage('docker-redis-v2') {
      steps {
        sh '''docker_redis=`docker ps -a | grep redisv1 | awk \'{print $1}\'`
if [ -z $docker_redis ];then
  docker run -it -d --rm --name redisv2 redis:v2
else
  docker stop redisv1 
  docker run -it -d --rm --name redisv2 redis:v2
fi'''
      }
    }

  }
}