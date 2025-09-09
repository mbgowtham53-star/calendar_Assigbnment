pipeline {
    agent { label 'August_label' }

    tools {
        jdk 'JDK17'
        maven 'Maven3'
    }

    environment {
        SERVER_IP_1 = "172.31.30.142"
        SERVER_IP_2 = "172.31.29.157"
        USER_NAME   = "ubuntu"
        TMP_DIR     = "/tmp/App/"
        TOMCAT_DIR  = "/opt/tomcat/webapps/"
        REPO_DIR    = "${WORKSPACE}/Apache_Stratos_Tomcat_Applications-fork"
        WAR_FILE    = "${REPO_DIR}/*.war"
    }

    stages {
        stage('Build') {
    steps {
        echo "Cloning repository..."
        sh """
            if [ -d "${REPO_DIR}" ]; then
                cd ${REPO_DIR} && git pull
            else
                git clone -b master https://github.com/anandg7259/Apache_Stratos_Tomcat_Applications-fork.git
            fi
        """
    }
}


        stage('Deploy to Tomcat1') {
            steps {
                echo "Deploying WAR to Tomcat1..."
                sh """
            
                    ssh ${USER_NAME}@${SERVER_IP_1} "mkdir -p ${TMP_DIR}"
                    scp $WAR_FILE ${USER_NAME}@${SERVER_IP_1}:${TMP_DIR}
                    ssh ${USER_NAME}@${SERVER_IP_1} "sudo mv ${TMP_DIR}/*.war ${TOMCAT_DIR}"
                """
            }
        }

        stage('Deploy to Tomcat2') {
            steps {
                echo "Deploying WAR to Tomcat2..."
                sh """
                
                    fi
                    ssh ${USER_NAME}@${SERVER_IP_2} "mkdir -p ${TMP_DIR}"
                    scp $WAR_FILE ${USER_NAME}@${SERVER_IP_2}:${TMP_DIR}
                    ssh ${USER_NAME}@${SERVER_IP_2} "sudo mv ${TMP_DIR}/*.war ${TOMCAT_DIR}"
                """
            }
        }

        stage('Test') {
    steps {
        echo "Waiting for Tomcat to start the applications..."
        // sleep for 20 seconds
        sleep(time: 20, unit: 'SECONDS')

        echo "Verifying Calendar application on Tomcat1..."
        sh 'curl -f -I http://${SERVER_IP_1}:8080/Calendar/ || exit 1'

        echo "Verifying Calendar application on Tomcat2..."
        sh 'curl -f -I http://${SERVER_IP_2}:8080/Calendar/ || exit 1'
    

            }
        }
    }
}
