def code  = 'test';
def notify(message, stickerId) {
    def token = "vev2IewXbmQjYDpzKrFHmDBAEmLseBZqDR5H1kATykA";
    def buildNo = env.BUILD_NUMBER;

    def url = "https://notify-api.line.me/api/notify";
    def lineMessage = "Node.12-${message} ${buildNo} \r\n";
	sh "curl ${url} -H 'Authorization: Bearer ${token}' -F 'message=${lineMessage}' -F 'stickerId=${stickerId}' -F 'stickerPackageId=1'";
}

pipeline {
    agent any
    environment {
        NEW_VERSION = '1.0.0'
        CI = 'true'
    }
    stages {
        stage('Staging') {
            steps {
                sh 'kubectl apply -f deployment.yml';
            }
        }
        stage('Check Staging Ready') {
            steps {
                script {
                    try {
                        sleep 50 
                        code = sh(returnStdout: true, script: "curl -o /dev/null -s -w '%{http_code}' http://194.233.73.42:5050/alive").trim()
                        echo "HTTP response status code: ${code}"
                    } catch (Exception e){
                   	    notify('Something wrong (stage[Check Staging Ready]) 🤳🤳', '9')
                    }
                }
            }
        }
    }
}
