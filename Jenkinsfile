pipeline {
  agent any

  stages {
    stage('git scm update') {
      steps {
        git url: 'https://github.com/namwoojin87/ktcloudinfrajenkins.git',
            branch: 'main'
      }
    }

    stage('docker image build') {
      steps {
        sh 'docker build -t moogee87/ktcloudinfra4:0727 .'
      }
    }

    stage('push to docker hub') {
      steps {
        sh 'docker push moogee87/ktcloudinfra4:0727'
      }
    }

    stage('copy deploy.yml to master') {
      steps {
        sh '''
          ansible master -m copy \
            -a "src=deploy.yml dest=/root/deploy.yml owner=root group=root mode=0644"
        '''
      }
    }

    stage('deploy using kubernetes') {
      steps {
        sh '''
          ansible master -m shell \
            -a "kubectl --kubeconfig=/etc/kubernetes/admin.conf apply -f /root/deploy.yml"
        '''
      }
    }
  }

  post {
    success {
      echo 'Pipeline succeeded!'
    }
    failure {
      echo 'Pipeline failed!'
    }
  }
}
