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
            # Create the application directory if it doesn't exist
            sudo mkdir -p ${appDir}
            sudo chown -R jenkins:jenkins ${appDir}

            # Sync the application files to the EC2 instance
            rsync -av --delete 
            --exclude='.git' 
            --exclude='node_modules' 
            ./ ${appDir}/

            # Install dependencies and build the React application
            cd ${appDir}
            sudo npm install
            sudo npm run build
            sudo fuser -k 3000/tcp || true
            
            npm run start

        """
    }
}