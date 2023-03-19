pipeline {
    agent any
    stages {
        stage('Docker start') {
            steps {
                sh "REPO_ROOT=/var/jenkins_home/workspace/project-drupal docker-compose up -d"
            }
        }
        stage('Composer') {
            steps {
                sh "docker-compose exec -it webserver composer --working-dir=/var/www install"
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
