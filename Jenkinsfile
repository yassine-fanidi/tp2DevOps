pipeline {
    agent any

    environment {
        // Injection sécurisée de vos identifiants Jenkins pour Docker Hub
        DOCKER_REGISTRY_CREDS = credentials('cc7d5e1e-06db-44a3-a433-48a90504cebf')
    }

    stages {
        // Stage 1 : Récupération du code depuis GitHub
        stage('Cloning Git') {
            steps {
                echo 'Clonage du dépôt Git...'
                git credentialsId: 'c86db311-c2f9-40a0-9788-ebc9a9e907e1',
                    url: 'https://github.com/yassine-fanidi/tp2DevOps',
                    branch: 'master'
            }
        }

        // Stage 2 : Construction de l'image Docker
        stage('Building image') {
            steps {
                echo 'Construction de l\'image Docker...'
                bat "docker build -t yassinedevelopper/tp2devops:Pipeline2-%BUILD_NUMBER% ."
            }
        }

        // Stage 3 : Test de l'image (Vérification de la présence de l'image locale)
        stage('Test image') {
            steps {
                echo 'Vérification de l\'existence de l\'image créée...'
                bat "docker images yassinedevelopper/tp2devops:Pipeline2-%BUILD_NUMBER%"
            }
        }

        // Stage 4 : Authentification et Publication sur Docker Hub
        stage('Publish Image') {
            steps {
                echo 'Publication de l\'image sur Docker Hub...'
                bat """
                    echo %DOCKER_REGISTRY_CREDS_PSW% | docker login -u yassinedevelopper --password-stdin
                    docker push yassinedevelopper/tp2devops:Pipeline2-%BUILD_NUMBER%
                    docker logout
                """
            }
        }
    }
}
