pipeline {
    agent any
    
    tools {
        maven 'Maven-3.9'
    }
    
    environment {
        PROJECT_NAME = 'Spring PetClinic'
    }
    
    stages {
        stage('📥 Clone Repository') {
            steps {
                echo "Clonage du projet ${PROJECT_NAME}..."
                git branch: 'main', url: 'https://github.com/mtgfz/devops-tp-2026.git'
            }
        }
        
        stage('🔍 Check Environment') {
            steps {
                echo 'Vérification de l\'environnement...'
                bat 'java -version'
                bat 'mvn -version'
            }
        }
        
        stage('🔨 Compile') {
            steps {
                echo 'Compilation du projet...'
                bat 'mvn clean compile'
            }
        }
        
        stage('🧪 Run Tests') {
            steps {
                echo 'Exécution des tests unitaires...'
                bat 'mvn test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }
        
        stage('📦 Package') {
            steps {
                echo 'Création du JAR...'
                bat 'mvn package -DskipTests'
            }
        }
        
        stage('📊 SonarQube Analysis') {
    	     steps {
        	echo 'Analyse de la qualité du code avec SonarQube...'
        	bat 'mvn sonar:sonar -Dsonar.projectKey=petclinic -Dsonar.host.url=http://localhost:9000 -Dsonar.login=admin -Dsonar.password=Lemt123@@@'
    }
}
        
        stage('💾 Archive Artifacts') {
            steps {
                echo 'Archivage des artefacts...'
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            }
        }
    }
    
    post {
        success {
            echo '✅ Build réussi ! Le JAR a été créé avec succès.'
        }
        failure {
            echo '❌ Build échoué ! Consultez les logs ci-dessus.'
        }
        always {
            echo "Build terminé à ${new Date()}"
        }
    }
}