pipeline {
    agent any

    environment {
        DB_URL = 'jdbc:mysql://localhost:3306/testdb'
        DB_USER = 'root'
        DB_PASS = 'qwerty123' // или credentials()
        LIQUIBASE_HOME = '/opt/liquibase'
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/iroot1983/liquibase-mysql-demo.git'
            }
        }

        stage('Run Liquibase') {
            steps {
                sh """
                    ${LIQUIBASE_HOME}/liquibase \
                        --changeLogFile=changelog/db.changelog-master.xml \
                        --url=${DB_URL} \
                        --username=${DB_USER} \
                        --password=${DB_PASS} \
                        update
                """
            }
        }
    }
}
