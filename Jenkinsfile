

def test_Variable = "test"

pipeline {
    // agent any 
    // agent { label 'aws' }
    agent any     

    parameters {
        booleanParam(defaultValue: false,
                     description: 'If checked downstream pipeline is also',
                    name: 'build_downstream'
        )
        string(name: 'STATEMENT',
            defaultValue: 'hello; ls /', description: 'What should I say?'
        )
    }

    tools {
        maven 'maven_3.8.4'
        jdk 'jdk-8'
    }

    stages {
        stage('Introduction and Pre Checks') {
            steps {
                echo 'This is a minimal pipeline.'
                script {
                    sh 'df -h'
                    sh 'java -version'
                    sh 'mvn --version'
                }
            }
        }
        stage('build') {
            steps {
                echo 'This is the build step'
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('unit test') {
            steps {
                echo 'This is the build step'
                sh 'mvn test'
            }
        }
        stage('build downstream') {
            when{
                anyOf{
                    expression (return env.build_downstream==true)
                }
            }
            steps {
                echo 'This is the downstream build step'
            }
        }
        
    }
    post {
        failure {
            echo "Build POST  FAILURE action "
        }
        always {
            echo "Build POST action"
        }
    }
}
