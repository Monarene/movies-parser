def imageName = 'mlabouardy/movies-parser'
def myImageName = 'monarene/movie-parser'

node(''){
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

def commitID() {
    sh 'git rev-parse HEAD > .git/commitID'
    def commitID = readFile('.git/commitID').trim()
    sh 'rm .git/commitID'
    commitID
}

