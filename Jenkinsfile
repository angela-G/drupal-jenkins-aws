pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
    }
    stages {
        stage('Start Containers') {
            steps {
                bat "docker-compose up -d"
            }
        }
        stage('Install Dependencies') {
            steps {
                bat "docker-compose exec -w /var/www webserver composer install"
            }
        }
        stage('Static Analysis') {
            steps {
               bat "docker-compose exec -w /var/www webserver ./vendor/bin/phpcs --standard=Drupal,DrupalPractice web/modules/custom web/themes/custom"
            }
        }
        stage('Unit tests') {
            steps {
                bat "docker-compose exec -w /var/www/web/core webserver cp phpunit.xml.dist phpunit.xml"
                bat "docker-compose exec -w /var/www webserver ./vendor/bin/phpunit -c /var/www/web/core/phpunit.xml web/modules/custom"
            }
        }
        stage('Build') {
            steps {
                bat 'docker login -u DOCKERHUB_CREDENTIALS_USR -p DOCKERHUB_CREDENTIALS_PSW docker.io'
                bat 'docker-compose build'
                bat 'docker-compose push'
            }
        }
    }
    post {
      always {
        bat "docker-compose down"
      }
    }
}
