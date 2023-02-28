def imageName = 'mlabouardy/movies-parser'
def myImageName = 'monarene/movie-parser'

node(''){

    try {

    stage('Checkout'){
        checkout scm
    }

    def imageTest= docker.build("${imageName}-test", "-f Dockerfile.test .")


    
    stage('Pre-integration Tests'){
        parallel(
            'Quality Tests': {
                imageTest.inside{
                    sh 'echo "Quality tests"'
                }
            },
            'Unit Tests': {
                imageTest.inside{
                    sh 'echo "Unit test"'
                }
            },
            'Security Tests': {
                imageTest.inside{
                    sh 'echo "Security tests"'
                }
            }
        )
    }

    stage('Build'){
        dockerImage = docker.build(myImageName)
    }

    stage('Push'){
        withDockerRegistry([credentialsId: "dockerhub", url: "" ]) {
        dockerImage.push(commitID())
        
        if (env.BRANCH_NAME == 'develop') {
            dockerImage.push('develop')
        }
        
        }
        
    }

     } 
    catch(e){
        currentBuild.result = 'FAILED'
        throw e
    } finally {
        notifySlack(currentBuild.result)
    }


}

def notifySlack(String buildStatus){
    buildStatus =  buildStatus ?: 'SUCCESSFUL'
    def colorCode = '#FF0000'
    def subject = "Name: '${env.JOB_NAME}'\nStatus: ${buildStatus}\nBuild ID: ${env.BUILD_NUMBER}"
    def summary = "${subject}\nMessage: ${commitMessage()}\nAuthor: ${commitAuthor()}\nURL: ${env.BUILD_URL}"

    if (buildStatus == 'STARTED') {
        colorCode = '#546e7a'
    } else if (buildStatus == 'SUCCESSFUL') {
        colorCode = '#2e7d32'
    } else {
        colorCode = '#c62828c'
    }

    slackSend (color: colorCode, message: summary)
}


def commitAuthor(){
    sh 'git show -s --pretty=%an > .git/commitAuthor'
    def commitAuthor = readFile('.git/commitAuthor').trim()
    sh 'rm .git/commitAuthor'
    commitAuthor
}

def commitID() {
    sh 'git rev-parse HEAD > .git/commitID'
    def commitID = readFile('.git/commitID').trim()
    sh 'rm .git/commitID'
    commitID
}

def commitMessage() {
    sh 'git log --format=%B -n 1 HEAD > .git/commitMessage'
    def commitMessage = readFile('.git/commitMessage').trim()
    sh 'rm .git/commitMessage'
    commitMessage
}

