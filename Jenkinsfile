pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'mvn -f hello-app/pom.xml -B -DskipTests clean package'
            }
            post {
                success {
                    echo "Now Archiving the Artifacts....."
                    archiveArtifacts artifacts: '**/*.jar'
                }
            }
        }
        stage('Dependency Check') {
            steps {
                script {
                    def dependencyCheckHome = tool name: 'Dependency-Check', type: 'org.jenkinsci.plugins.DependencyCheck.tools.DependencyCheckInstallation'
                    sh "${dependencyCheckHome}/bin/dependency-check.sh --project MyProject --out . --scan hello-app/target"
                }
            }
        }
        stage('Test') {
            steps {
                sh 'mvn -f hello-app/pom.xml test'
            }
            post {
                always {
                    junit 'hello-app/target/surefire-reports/*.xml'
                }
            }
        }
    }
}

