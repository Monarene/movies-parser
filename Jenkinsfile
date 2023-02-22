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
        sh 'go test'} 
        }

}
