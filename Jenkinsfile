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
    tools {
        dotnetsdk 'dotnet-9.0.0'
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
                dotnetBuild(
                    configuration: 'Release',
                    project: 'src/Presentation/Nop.Web/Nop.Web.csproj'
                )
            }
        }
        stage('clean') {
            steps {
                dotnetClean(
                    outputDirectory: 'published'
                )
            }
        }
        stage('publish') {
            steps {
                dotnetPublish(
                    configuration: 'Release',
                    outputDirectory: 'published',
                    project: 'src/Presentation/Nop.Web/Nop.Web.csproj'
                )
            }
        }
    }
    post {
        success {
            zip(
                zipFile: './published.zip',
                archive: true,
                dir: './published',
                overwrite: true
            )
            echo 'Build and published successfully'
        }
        failure {
            echo 'Build failed'
        }
    }
}