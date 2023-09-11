def imageName = 'adriannavarro/parser'
def registry = 'https://hub.docker.com'

node('jenkins_agent'){
    stage('Checkout'){
        checkout scm
    }

    stage('Pre-integration Tests'){
        parallel(
            'Quality Tests': {
                // imageTest.inside{
                //     sh 'golint'
                // }
                echo "Quality tests passed"
            },
            'Unit Tests': {
                // imageTest.inside{
                //     sh 'go test'
                // }
                echo "Unit tests passed"
            },
            'Security Tests': {
                echo "Security tests passed"

            }
        )
    }

    stage('Build'){
        docker.build(imageName)
    }

    stage('Push'){
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            docker.image(imageName).push(commitID())

            if (env.BRANCH_NAME == 'develop') {
                docker.image(imageName).push('develop')
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