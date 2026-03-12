node {

 stage('Checkout') {
        git branch: 'main', url: 'https://github.com/Mafirrr/WsDevops.git'
    }

    stage('Build') {
        docker.image('php:8.4-cli').inside('--network host --dns 8.8.8.8 --dns 8.8.4.4 --entrypoint="" -u root') {
            sh '''
            apt-get update
            apt-get install -y git unzip libzip-dev curl
            docker-php-ext-install zip

            curl -sS https://getcomposer.org/installer | php
            mv composer.phar /usr/local/bin/composer

            composer install --ignore-platform-req=ext-gd
            '''
        }
    }

    stage('Testing') {
        docker.image('ubuntu:22.04').inside('--entrypoint="" -u root') {
            sh '''
            echo "Ini adalah test"
            '''
        }
    }

    stage('Deploy') {
    docker.image('agung3wi/alpine-rsync:1.1').inside('--entrypoint="" -u root') {
        sshagent(credentials: ['ssh-prod']) {
            sh '''
            mkdir -p ~/.ssh
            ssh-keyscan -H $PROD_HOST >> ~/.ssh/known_hosts

            rsync -rav --delete \
                --exclude=.env \
                --exclude=storage \
                --exclude=.git \
                ./ firr@$PROD_HOST:/home/nixie/m6/larajenkins/
            '''
        }
    }
    }

}
