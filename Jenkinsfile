node {
    def appDir = '/var/www/react_app'

    stage('Clean Workspace') {
        echo 'Cleaning workspace...'
        deleteDir()
    }

    stage('Checkout Code') {
        echo 'Checking out code from Git...'
        git (
            branch: 'main',
            url: 'https://github.com/arghjain29/react_ec2_jenkins_pipeline'
        )
    }

    stage('Deploy Application') {
        echo 'Deploying application to EC2...'
        sh """
        sudo mkdir -p ${appDir}
        sudo chown -R jenkins:jenkins ${appDir}

        rsync -av --delete \
        --exclude='.git' \
        --exclude='node_modules' \
        ./ ${appDir}/

        cd ${appDir}
        sudo npm install
        sudo npm run build
        sudo npm run preview -- --host 0.0.0.0 --port 5173 > /tmp/react_app.log 2>&1 &
        """
    }
}