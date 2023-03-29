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
                bat "docker-compose exec -it webserver COMPOSER_PROCESS_TIMEOUT=2000 && composer --working-dir=/var/www install"
            }
        }
        // stage('Static Analysis') {
        //     sh "docker-compose exec webserver -T ./vendor/bin/phpcs --standard=Drupal,DrupalPractice"
        // }
        // stage('Unit tests') {
        //     sh "docker-compose exec webserver -T ./vendor/bin/phpunit -c phpunit.xml"
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
