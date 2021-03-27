pipeline{
    agent{
        docker{
            image "maven:3.6.0-jdk-13"
            label "agent01"
        }
    }
    options{
        buildDiscarder(logRotator(numToKeepStr: '5'))
        disableResume()
        disableConcurrentBuilds()
        timestamps()
        timeout(time: 1, unit: 'HOURS')
    }
    stages{
        stage('Pre Check'){
            steps{
                sh 'mvn --version'
            }
        }
        stage('Build'){
            steps{
                sh 'mvn clean package'
            }
        }
        stage('Run'){
            steps{
                sh 'java -cp ${WORKSPACE}/target/maven-pipeline-docker-1-1.0.jar myTest.com.App'
            }
        }
    }
    post{
        always{
            echo "Archive Artifacts"
	    archiveArtifacts allowEmptyArchive: false, artifacts: 'target/*.jar', fingerprint: true, defaultExcludes: false, followSymlinks: false, onlyIfSuccessful: true
        }
        cleanup{
	   echo "Workspace cleanup"
 	   cleanWs()
        }
    }
}
