pipeline {
	agent {label "new-node"}
	environment {
		Docker_Cred=credentials('Docker_Cred')
	}
	stages {
		stage ('SCM checkout') {
			steps {
				git branch:'main', url:'https://github.com/asifkhazi/docker-multistagebuild-java.git'
			}
		}
		stage('SonarQube Analysis Stage') {
                        steps{ 
                             sh ''' mvn clean verify sonar:sonar \
  				-Dsonar.projectKey=docker-multistagebuild-java \
  				-Dsonar.projectName='docker-multistagebuild-java' \
  				-Dsonar.host.url=http://18.61.160.246:9000 \
  				-Dsonar.token=sqp_e5853f9af260acad86c9d331566f4566027d6d30 '''
            		}
        	}
		stage ('Build and Create docker image') {
			steps {
				sh 'docker build -t ${Docker_Cred_USR}/tomcatjar:${BUILD_ID} -f Dockerfile .'
			}
		}
		stage ('Push image to artifactory') {
			steps {
				sh 'docker login -u ${Docker_Cred_USR} -p ${Docker_Cred_PSW}'
				sh 'docker push ${Docker_Cred_USR}/tomcatjar:${BUILD_ID}'
			}
		}
		stage ('Deploy') {
			steps {
				sh 'docker run --name cont-${BUILD_ID} ${Docker_Cred_USR}/tomcatjar:${BUILD_ID}'
			}
		}
		
	}
}
