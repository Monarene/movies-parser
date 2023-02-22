def imageName = 'mlabouardy/movies-parser'

node(''){
    stage('Checkout'){
        checkout scm
    }

    stage('Quality Tests'){
        def imageTest= docker.build("${imageName}-test", "-f Dockerfile.test .")
        imageTest.inside{
            sh 'echo "blah blah"' }
        } 

    stage('Unit Tests'){
        imageTest.inside{
        sh 'this is a go test'} 
        }

    stage('Security Checks'){
        imageTest.inside{
        sh 'this is a security check with nancy'} 
        }

}
