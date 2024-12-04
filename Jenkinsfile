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
                sh 'dotnet build -c Release src/Presentation/Nop.Web/Nop.Web.csproj'
            }
        }
        stage('publish') {
            steps {
                sh 'mkdir published'
                sh 'dotnet publish -c Release -o ./published/ src/Presentation/Nop.Web/Nop.Web.csproj'
            }
        }
    }
    post {
        success {
            zip zipFile : './published.zip',
                archive : true,
                dir: './published',
                overwrite : true
            echo 'Build and published successfully'
        }
        failure {
            echo 'Build failed'
        }
    }
}