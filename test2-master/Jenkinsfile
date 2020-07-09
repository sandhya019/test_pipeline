pipeline {
    agent any
    tools {
      maven 'Maven3'
    }
    stages{
        stage('Build'){
            steps{
                sh script: 'mvn clean package'
            }
        }
        stage('Upload jar to Nexus'){
            steps{
                script{
                    def mavenPom = readMavenPom file:'pom.xml'
                    def nexusRepoName = mavenPom.version.endsWith("SNAPSHOT")?"testapp-snapshot":"testapp-release"
                    nexusArtifactUploader artifacts: [[
                    artifactId: 'test2',
                    classifier: '',
                    file: "target/test2-${mavenPom.version}.jar",
                    type: 'jar']],
                    credentialsId: 'nexus3',
                    groupId: 'org.example',
                    nexusUrl: '172.31.0.123:8081',
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    repository: nexusRepoName,
                    version: "${mavenPom.version}"
                }
            }
        }
    }
}