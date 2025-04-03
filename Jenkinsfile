pipeline {
    agent any

    environment {
        DB_URL = 'jdbc:mysql://localhost:3306/testdb'
        DB_USER = 'root'
        DB_PASS = 'qwerty123'
        LIQUIBASE_HOME = '/opt/liquibase'
    }

    stages {
        stage('Apply Migration') {
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

        stage('Manual Rollback Decision') {
            steps {
                script {
                    def userInput = input(
                        id: 'rollbackConfirm', message: 'Откатить миграцию?',
                        parameters: [
                            choice(name: 'ROLLBACK', choices: ['Нет', 'Да'], description: 'Rollback последней миграции?')
                        ]
                    )

                    if (userInput == 'Да') {
                        echo "⚠️ Выполняем rollback..."
                        sh """
                            ${LIQUIBASE_HOME}/liquibase \
                              --changeLogFile=changelog/db.changelog-master.xml \
                              --url=${DB_URL} \
                              --username=${DB_USER} \
                              --password=${DB_PASS} \
                              rollbackCount 1
                        """
                    } else {
                        echo "✅ Продолжаем без отката."
                    }
                }
            }
        }
    }

    post {
        success {
            echo "🎉 Pipeline завершён успешно."
        }
        failure {
            echo "❌ Ошибка в pipeline."
        }
    }
}
