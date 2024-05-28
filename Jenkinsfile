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
				sh 'docker run -it --name cont-${BUILD_ID} ${Docker_Cred_USR}/tomcatjar:${BUILD_ID}'
			}
		}
		
	}
}
