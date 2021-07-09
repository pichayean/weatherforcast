def code  = '';
def notify(message, stickerId) {
    def token = "vev2IewXbmQjYDpzKrFHmDBAEmLseBZqDR5H1kATykA";
    def buildNo = env.BUILD_NUMBER;

    def url = "https://notify-api.line.me/api/notify";
    def lineMessage = "weatherforecast-${message}:buildNo-${buildNo} \r\n";
	sh "curl ${url} -H 'Authorization: Bearer ${token}' -F 'message=${lineMessage}' -F 'stickerId=${stickerId}' -F 'stickerPackageId=1'";
}

def getDockerTag(){
    def tag  = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}

pipeline {
    agent any
    environment {
        NEW_VERSION = '1.0.0'
        CI = 'true'
        DOCKER_TAG = getDockerTag()
    }
    stages {
        stage('Build') {
            steps {
                sh 'docker build --no-cache -t weatherforecast:${DOCKER_TAG} .';
            }
        }
        stage('Deployment to kube') {
            steps {
                sh "chmod +x sedtag.sh"
                sh "./sedtag.sh ${DOCKER_TAG}"
                sh 'kubectl apply -f app-deployment.yml';
            }
        }
        stage('Check Staging Ready') {
            steps {
                script {
                    try {
                        sleep 40 
                        code = sh(returnStdout: true, script: "curl -o /dev/null -s -w '%{http_code}' http://194.233.73.42:3000/WeatherForecast/alive").trim()
                        echo "HTTP response status code: ${code}"
                    } catch (Exception e){
                   	    notify('Something wrong (stage[Check Staging Ready]) 🤳🤳', '9')
                    }
                }
            }
        }
        stage('AppReady') {
            steps {
                script {
                    if (code == '200') {
                        notify('Deploy new version Success 😜💖', '3')
                    } else {
                        echo 'I execute elsewhere'
                    }
                }
            }
        }
    }
}
