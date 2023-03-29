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
                // bat "docker-compose exec -it webserver composer --working-dir=/var/www install"
                bat "docker-compose exec -w /var/www webserver composer install"
            }
        }
        stage('Static Analysis') {
            steps {
                // bat "docker-compose exec -w /var/www webserver ./vendor/bin/phpcs -d memory_limit=512M --standard=Drupal,DrupalPractice web/modules/custom web/themes/custom"
                bat "docker-compose exec -w /var/www webserver ./vendor/bin/phpcs --standard=Drupal,DrupalPractice web/modules/custom web/themes/custom"
            }
        }
        // stage('Unit tests') {
        //     sh "docker-compose exec webserver -T ./vendor/bin/phpunit -c web/phpunit.xml"
        // }
        // stage('Database sync') {
        //     sh "drush cim -y"
        //     // sanitize, updb, cim, cr, file sync..
        // }
        // stage('Functional tests') {
        //     sh "docker-compose exec webserver -T ./vendor/bin/behat --config tests/behat/behat.yml"
        // }
    }
    // post {
    //   always {
    //     sh "docker-compose down"
    //   }
    // }
}
