pipeline {
    agent any
    stages {
        stage('Docker start') {
            steps {
                bat "docker-compose up -d"
            }
        }
        stage('Composer') {
            steps {
                bat "docker-compose exec -w /var/www webserver composer install"
            }
        }
        stage('Static Analysis') {
            steps {
               bat "docker-compose exec -w /var/www webserver ./vendor/bin/phpcs --standard=Drupal,DrupalPractice web/modules/custom web/themes/custom"
            }
        }
        // stage('Unit tests') {
        //     steps {
        //         bat "docker-compose exec -w /var/www/web/core webserver cp phpunit.xml.dist phpunit.xml"
        //         bat "docker-compose exec -w /var/www webserver ./vendor/bin/phpunit -c /var/www/web/core/phpunit.xml web/modules/custom"
        //     }
        // }
        stage('Database sync') {
            steps {
                bat "docker-compose exec -w /var/www/web webserver ./vendor/bin/drush updb -y && ./vendor/bin/drush cim -y && ./vendor/bin/drush cr"
            }
        }
        // stage('Functional tests') {
        //     sh "docker-compose exec webserver -T ./vendor/bin/behat --config tests/behat/behat.yml"
        // }
    }
    post {
      always {
        bat "docker-compose down"
      }
    }
}
