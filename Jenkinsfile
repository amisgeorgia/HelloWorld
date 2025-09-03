pipeline {
    agent any

    parameters {
        choice(name: 'OS', choices: ['Linux', 'Windows', 'MacOS'], description: 'Choisir OS de build')
    }

    stages {
        stage('Clone') {
            steps {
               git url: 'https://github.com/amisgeorgia/HelloWorld.git', branch: 'main', credentialsId: '6c6543d4-7a3d-4b72-9204-0f14e008f7b9'
            }
        }
        stage('Clean') {
            steps {
                bat '"C:/apache-maven-3.9.9/bin/mvn" clean'
            }
        }
        stage('Test') {
            steps {
                bat '"C:/apache-maven-3.9.9/bin/mvn" test'
            }
        }
        stage('Package') {
            steps {
                bat '"C:/apache-maven-3.9.9/bin/mvn" package'
            }
        }
        // stage('Deploy') {
        //     when {
        //         expression { 
        //             // Ne déployer que si la configuration distributionManagement existe
        //             return fileExists('pom.xml') && 
        //                 readFile('pom.xml').contains('distributionManagement')
        //             }
        //         }
        //         steps {
        //             bat '"C:/apache-maven-3.9.9/bin/mvn" deploy'
        //         }
        // }
        stage('SonarQube analysis') {
            // steps {
            //    withSonarQubeEnv('MonServeurSonarQube') { // Remplacez 'MonServeurSonarQube' par le nom que vous avez choisi
            //         bat '"C:/apache-maven-3.9.9/bin/mvn" sonar:sonar'
            //     }
            // }
            steps {
                withSonarQubeEnv('MonServeurSonarQube') {
                    withCredentials([string(credentialsId: 'sonarqube-token', variable: 'SONAR_TOKEN')]) {
                        bat """
                        "C:/apache-maven-3.9.9/bin/mvn" sonar:sonar ^
                          -Dsonar.projectKey=HelloWorld ^
                          -Dsonar.host.url=http://localhost:9000 ^
                          -Dsonar.token=%SONAR_TOKEN%
                        """
                    }
                }
            }
        }
    }
}
