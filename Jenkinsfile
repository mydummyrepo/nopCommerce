pipeline {
    agent {
        label 'dotnet'
    }
    options { 
        timeout(time: 20, unit: 'MINUTES')
    }
    triggers {
        pollSCM('H * * * 1-5')
    }
    stages {
        stage('SCM') {
            steps {
                git url: 'https://github.com/mydummyrepo/nopCommerce.git',
                branch: 'develop'
            }
        }
        stage('build') {
            steps {
                dotnetBuild {
                    configuration 'Release'
                    project 'src/Presentation/Nop.Web/Nop.Web.csproj'
                    runtime '8.0.10'
                }
            }
        }
        stage('publish') {
            steps {
                dotnetPublish {
                    configuration 'Release'
                    project 'src/Presentation/Nop.Web/Nop.Web.csproj'
                    runtime '8.0.10'
                    outputDirectory 'published'
                }
            }
        }
    }
    post {
        success {
            echo 'Build and published successfully'
        }
        failure {
            echo 'Build failed'
        }
    }
}