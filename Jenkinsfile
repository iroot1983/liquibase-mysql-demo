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

        stage('Выбор rollback') {
            steps {
                script {
                    def userInput = input(
                        id: 'rollbackChoice',
                        message: 'Откатить какую миграцию?',
                        parameters: [
                            choice(
                                name: 'ROLLBACK',
                                choices: ['Ничего', 'Users', 'Orders'],
                                description: 'Выберите, что откатить'
                            )
                        ]
                    )

                    if (userInput == 'Users') {
                        echo "🔁 Откат users..."
                        sh """
                            ${LIQUIBASE_HOME}/liquibase \
                              rollbackOneChangeSet \
                              --changeLogFile=changelog/db.changelog-master.xml \
                              --url=${DB_URL} \
                              --username=${DB_USER} \
                              --password=${DB_PASS} \
                              --changeSetId=1 \
                              --changeSetAuthor=dev \
                              --changeSetPath=changelog/001_create_users_table.xml
                        """
                    } else if (userInput == 'Orders') {
                        echo "🔁 Откат orders..."
                        sh """
                            ${LIQUIBASE_HOME}/liquibase \
                              rollbackOneChangeSet \
                              --changeLogFile=changelog/db.changelog-master.xml \
                              --url=${DB_URL} \
                              --username=${DB_USER} \
                              --password=${DB_PASS} \
                              --changeSetId=2 \
                              --changeSetAuthor=dev \
                              --changeSetPath=changelog/002_create_orders_table.xml
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
