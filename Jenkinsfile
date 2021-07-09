def code  = 'test';
def notify(message, stickerId) {
    def token = "vev2IewXbmQjYDpzKrFHmDBAEmLseBZqDR5H1kATykA";
    def buildNo = env.BUILD_NUMBER;

    def url = "https://notify-api.line.me/api/notify";
    def lineMessage = "weatherforecast-${message}:buildNo-${buildNo} \r\n";
	sh "curl ${url} -H 'Authorization: Bearer ${token}' -F 'message=${lineMessage}' -F 'stickerId=${stickerId}' -F 'stickerPackageId=1'";
}

pipeline {
    agent any
    environment {
        NEW_VERSION = '1.0.0'
        CI = 'true'
    }
    stages {
        stage('Build') {
            steps {
                sh 'docker build -t weatherforecast:v2 .';
            }
        }
        stage('Deployment') {
            steps {
                sh 'kubectl apply -f ./k8s/deployment.yml';
            }
        }
        stage('Check Staging Ready') {
            steps {
                script {
                    try {
                        sleep 40 
                        code = sh(returnStdout: true, script: "curl -o /dev/null -s -w '%{http_code}' http://194.233.73.42:5050/alive").trim()
                        echo "HTTP response status code: ${code}"
                        notify('Deploy new version Success 😜💖', '3')
                    } catch (Exception e){
                   	    notify('Something wrong (stage[Check Staging Ready]) 🤳🤳', '9')
                    }
                }
            }
        }
        stage('AppReady') {
            steps {
                echo code;
            }
        }
    }
}