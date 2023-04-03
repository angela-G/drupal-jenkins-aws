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
                bat "docker-compose exec -w /var/www/web webserver ../vendor/bin/phpunit -c phpunit.xml --coverage-html ../build/coverage modules/custom"
            }
        }
        stage('Build') {
            steps {
                bat "docker login -u ${DOCKERHUB_CREDENTIALS_USR} -p ${DOCKERHUB_CREDENTIALS_PSW}"
                bat "docker build -t ${DOCKERHUB_CREDENTIALS_USR}/drupal-project:${BUILD_NUMBER} ."
                bat "docker push ${DOCKERHUB_CREDENTIALS_USR}/drupal-project:${BUILD_NUMBER}"
            }
        }
    }
    post {
      always {
        bat "docker logout"
        bat "docker-compose down"
        publishHTML([
            allowMissing: false,
            alwaysLinkToLastBuild: false,
            keepAll: false,
            reportDir: 'www/build/coverage',
            reportFiles: 'index.html',
            reportName: 'Coverage Report (HTML)',
            reportTitles: ''
        ])
      }
    }
}
