def tag
def version='1.0'
def branch='master'
def mobileVersion='1.0.4'
def specificCause = currentBuild.getBuildCauses('hudson.model.Cause$UserIdCause')
def notificationEmails="ganesan.kandasami@gmail.com"
def deployType='imobile-auth'
def imobileauth_deploy(){
	sh 'helm upgrade java-app /helm/java-app'
}
pipeline {
	agent any

	environment {
    DOCKERHUB_CREDENTIALS = credentials('fe99a365-14de-418f-98e4-b93030779645')
    }


	stages  {
		stage('clean-workspace'){
		    steps{
			   ///cleanWs()
				sh 'date'
			}
		}

		stage('pre-init') {
		    steps {
				script{
					tag = sh (
                            script: "echo \$(date +%b)_\$(date +%d)_\$(shuf -i 2000-65000 -n 1)",
                            returnStdout: true
                        ).trim()
				}
				echo  "Tag Name is : ${tag}"
				echo  "Deployment Type selected : ${deployType}"
		        sh "echo ${version}"
				sh "echo \"Tag Name for deployment :$tag\""


		    }
		}

		stage('git-checkout') {
		    steps {
                script {
				git branch: 'master',
   					 credentialsId: '9db7a662-10fb-49ba-8b48-b9adcd66236d',
   					 url: 'https://github.com/ganes891/springboot.git'
                 }
		    }
		}

		stage('maven-build') {
		    steps {

					//sh 'mvn clean install'
					sh 'whoami'

			}
		}

		stage('image-build') {
			steps {
				script {
						sh 'sudo docker build -t springboot-k8s .'
						//sh "sudo docker tag  springboot-k8s springboot-k8s:\"$tag\""
						sh "sudo docker tag  springboot-k8s ganesh891/springboot-k8s:\"$tag\""
						//sh "sudo docker login -u ganesh891 -p Liobarca@10 && docker push ganesh891/springboot-k8s:\"$tag\""

				}
			}
		}

				stage('docker-login-push') {
			steps {
				script {
						sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
						sh "sudo docker push ganesh891/springboot-k8s:\"$tag\""

				}
			}
		}
        stage ('helm-chart-release'){
            steps {
			    script {
				        sh 'sudo cp -r /root/.kube . && sudo cp -r  /root/.minikube .'
				        sh 'helm upgrade java-app /helm/java-app'
				}
            }
        }


    }
  	post {
        success {
            mail bcc: '', body: "<p>Hello Team</p><p>Production Deployment is Completed succesfully,which is triggered by <b> ${specificCause.userName} </b> for <b>optimus imobileauth backend services</b> with the following</p><table style=\"width: 100%;border: 1px solid black;	border-collapse: collapse;\"><tr style=\"border: 1px solid black;	border-collapse: collapse;\"><th width=\"30%\" align=\"left\" style=\"padding-left:10px; border: 1px solid black;	border-collapse: collapse;\">Deployment option</th><td width=\"70%\" align=\"left\" style=\"padding-left:10px;border: 1px solid black;	border-collapse: collapse;\"><span>${deployType}</span></td></tr><tr style=\"border: 1px solid black;	border-collapse: collapse;\"><th width=\"30%\" align=\"left\" style=\"padding-left:10px; border: 1px solid black;	border-collapse: collapse;\">Jira Id</th><td width=\"70%\" align=\"left\" style=\"padding-left:10px;border: 1px solid black;	border-collapse: collapse;\"><span>${params.JiraTicket}</span></td></tr><tr style=\"border: 1px solid black;	border-collapse: collapse;\"><th width=\"30%\" align=\"left\" style=\"padding-left:10px; border: 1px solid black;	border-collapse: collapse;\">Tag Name </th><td width=\"70%\" align=\"left\" style=\"padding-left:10px;border: 1px solid black;	border-collapse: collapse;\"><span>$tag</span></td></tr></table><br/><p style=\"font-family: sans-serif; font-size: 14px; font-weight: normal; margin: 0; Margin-bottom: 15px;\">Deployment status would be notified.- ${env.BUILD_URL}</p><p style=\"font-family: sans-serif; font-size: 14px; font-weight: normal; margin: 0; Margin-bottom: 15px;\">Thanks</p>", cc: '', charset: 'UTF-8', from: 'jenkins-notification@ganesh-dev.com', mimeType: 'text/html', replyTo: '', subject: "optimus production deployment completed succesfully : JIRA #${params.JiraTicket}", to: "${notificationEmails}";
        }
        failure {
            mail bcc: '', body: "<p>Hello Team</p><p>Production Deployment is Failure ,which is triggered by <b>${specificCause.userName}</b> for <b>optimus imobileauth backend services</b> with the following</p><table style=\"width: 100%;border: 1px solid black;	border-collapse: collapse;\"><tr style=\"border: 1px solid black;	border-collapse: collapse;\"><th width=\"30%\" align=\"left\" style=\"padding-left:10px; border: 1px solid black;	border-collapse: collapse;\">Deployment option</th><td width=\"70%\" align=\"left\" style=\"padding-left:10px;border: 1px solid black;	border-collapse: collapse;\"><span>${deployType}</span></td></tr><tr style=\"border: 1px solid black;	border-collapse: collapse;\"><th width=\"30%\" align=\"left\" style=\"padding-left:10px; border: 1px solid black;	border-collapse: collapse;\">Jira Id</th><td width=\"70%\" align=\"left\" style=\"padding-left:10px;border: 1px solid black;	border-collapse: collapse;\"><span>${params.JiraTicket}</span></td></tr><tr style=\"border: 1px solid black;	border-collapse: collapse;\"><th width=\"30%\" align=\"left\" style=\"padding-left:10px; border: 1px solid black;	border-collapse: collapse;\">Tag Name </th><td width=\"70%\" align=\"left\" style=\"padding-left:10px;border: 1px solid black;	border-collapse: collapse;\"><span>$tag</span></td></tr></table><br/><p style=\"font-family: sans-serif; font-size: 14px; font-weight: normal; margin: 0; Margin-bottom: 15px;\">Deployment status would be notified.- ${env.BUILD_URL}</p><p style=\"font-family: sans-serif; font-size: 14px; font-weight: normal; margin: 0; Margin-bottom: 15px;\">Thanks</p>", cc: '', charset: 'UTF-8', from: 'jenkins-notification@tatacommunications.com', mimeType: 'text/html', replyTo: '', subject: "optimus production deployment failed: JIRA #${params.JiraTicket}", to: "${notificationEmails}";
        }


    }
}
