pipeline {
  agent any
  tools {nodejs "node"}

	environment {
		DOCKER_IMAGE = 'chungnd/nodejs-mongodb'
		PROJECT_ID = 'divine-display-330317'
        CLUSTER_NAME = 'demo-cluster-0'
        LOCATION = 'asia-northeast1-a'
        CREDENTIALS_ID = 'key-gke'
		DOCKER_TAG="${GIT_BRANCH.tokenize('/').pop()}-${GIT_COMMIT.substring(0,7)}"		
	}
	
    stages {
	    
		stage('Test') {
		    steps {
				dir('app') 
				{
			    echo "Testing..."
					// sh '/usr/local/bin/phpunit/phpunit --version'
					// sh 'pwd'
					// sh 'pwd'
					// sh '/usr/local/bin/phpunit/phpunit .'
					// sh "npm install"
					// sh "npm test"
					// sh 'npm config ls'
					// sh 'npm install'
				}
		    }
	    }

	    stage('Build & Push Docker Image') {
			// agent { node {label 'main'}}

			// environment {
        	// 	DOCKER_TAG="${GIT_BRANCH.tokenize('/').pop()}-${GIT_COMMIT.substring(0,7)}"
      		// }
		    steps {
                echo "$DOCKER_TAG"
				
				withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerhub')]) {
            			sh "docker login -u chungnd -p ${dockerhub}"
				}
				dir('app') 
				{
					sh "ls"
					sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} . "
					sh "docker push ${DOCKER_IMAGE}:${DOCKER_TAG}"
					script {
						if (GIT_BRANCH ==~ /.*main.*/) {
							sh "docker tag ${DOCKER_IMAGE}:${DOCKER_TAG} ${DOCKER_IMAGE}:latest"
							sh "docker push ${DOCKER_IMAGE}:latest"
						}
					}
					sh "docker image rm ${DOCKER_IMAGE}:${DOCKER_TAG}"
				}
		    }
	    }
	    stage('Deploy to K8s') {
		    steps{
				script {
          			if (GIT_BRANCH ==~ /.*main.*/) {
						dir('k8s')
						{
							dir('demo-nodejs-mongodb-redis')
							{
								echo "Deployment started .. ."
								sh 'ls'
								sh 'pwd'
								sh 'echo ${DOCKER_TAG}'
								echo "Start deployment of nodejs-deployment.yaml"
								sh "sed -i 's/nodejs-mongodb:latest/nodejs-mongodb:${DOCKER_TAG}/g' nodejs-deployment.yaml"
								step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'nodejs-deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
								echo "Deployment Finished ..."
							}
						}
					  }
					else
					{
						echo 'Only merge Master that deploy in GKE'
					}
				}
			}
			
	    }
    }
}