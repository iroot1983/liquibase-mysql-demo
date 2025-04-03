pipeline {
    agent any

    environment {
        LIQUIBASE_HOME = '/opt/liquibase'
        DB_URL = 'jdbc:mysql://localhost:3306/testdb'
        DB_USER = 'root'
        DB_PASS = 'qwerty123'  // или используй Jenkins credentials
    }

    stages {
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

    post {
        success {
            echo "🎉 Миграции успешно применены!"
        }
        failure {
            echo "❌ Что-то пошло не так при применении миграций."
        }
    }
}
