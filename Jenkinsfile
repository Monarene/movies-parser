def imageName = 'mlabouardy/movies-parser'

node(''){
    stage('Checkout'){
        checkout scm
    }

    def imageTest= docker.build("${imageName}-test", "-f Dockerfile.test .")


    
    stage('Pre-integration Tests'){
        parallel(
            'Quality Tests': {
                imageTest.inside{
                    sh 'Quality tests'
                }
            },
            'Unit Tests': {
                imageTest.inside{
                    sh 'Unit test'
                }
            },
            'Security Tests': {
                imageTest.inside{
                    sh 'Security tests'
                }
            }
        )
    }

}
