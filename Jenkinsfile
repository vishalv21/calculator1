pipeline {
    // The “agent” section configures on which nodes the pipeline can be run.
    // Specifying “agent any” means that Jenkins will run the job on any of the
    // available nodes.

	agent any

    stages {
        stage('Git Pull') {
            steps {
                // Get code from a GitHub repository
                // Make sure to add your own git url and credentialsId
				git url: 'https://github.com/vishalv21/calculator1.git',
				branch: 'main'
			}
        }
        stage('Maven Build') {
            steps {
                // Maven build, 'sh' specifies it is a shell command
                sh 'mvn clean install'
            }
        }
        stage('Build Docker Images') {
                    steps {
                        sh 'docker build -t vishalvarma21/miniproj:latest .'
                    }
                }
                stage('Publish Docker Images') {
                    steps {
                        withDockerRegistry([ credentialsId: "dockerid", url: "" ]) {
                            sh 'docker push vishalvarma21/miniproj:latest'
                        }
                    }
                }
                stage('Clean Docker Images') {
                    steps {
                        sh 'docker rmi -f vishalvarma21/miniproj:latest'
                    }
                }
                stage('Deploy and Run Image'){
                            steps {
                                ansiblePlaybook becomeUser: null, colorized: true, disableHostKeyChecking: true, installation: 'Ansible', inventory: 'inventory', playbook: 'playbook.yml', sudoUser: null
                            }
                        }
    }
    post {
                always {
                    sh 'docker logout'
                }
            }
}