pipeline {
  agent {
    docker {
    	image 'rustynode:latest'
	    args '-u root --privileged'
    }
  }
  triggers {
    githubPush()
  }
  stages {
    stage('Clone') {
      steps {
	      sh '''
          cd /
          git clone https://github.com/yammyshep/jbuelow.com.git
          cd jbuelow.com
	      '''
	    }
    }
    stage('Build') {
    	steps {
	      sh '''
	      	cd /jbuelow.com
	      	npm install
	        npx parcel build src/index.html
	      '''
	    }
    }
    stage('Test') {
      steps {
        sh '''
	        cd /jbuelow.com
	        echo "not yet supported"
	      '''
      }
    }
    stage('Deploy') {
      when { branch 'main' }
      steps {
        sh '''
	        cd /jbuelow.com
          ssh -o "StrictHostKeyChecking no" -i /ssh/deploy_id_ecdsa jenkins-deploy-www@10.0.16.4 "rm -r /var/www/jbuelow.com/*"
          scp -o "StrictHostKeyChecking no" -i /ssh/deploy_id_ecdsa dist/* jenkins-deploy-www@10.0.16.4:/var/www/jbuelow.com/
        '''
      }
    }
  }
}

