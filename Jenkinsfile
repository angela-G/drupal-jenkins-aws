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
                bat 'docker login -u "DOCKERHUB_CREDENTIALS_USR" -p "DOCKERHUB_CREDENTIALS_PSW"'
              //  bat 'docker-compose build'
                bat 'docker build -t angela1g/drupal-project:$BUILD_NUMBER'
                bat 'docker push angela1g/drupal-project:$BUILD_NUMBER'
//                bat 'docker-compose push'
                // angela1g/drupal-project
            }
        }
    }
    post {
      always {
        bat "docker logout"
        bat "docker-compose down"
      }
    }
}
