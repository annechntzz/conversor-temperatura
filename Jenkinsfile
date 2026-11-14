
pipeline {
    agent any

    options {
        timestamps()
    }

    stages {
        stage('Verificar Python') {
            steps {
                bat '"C:/Users/annec/AppData/Local/Programs/Python/Python313/python.exe" --version'
            }
        }

        stage('Instalar dependencias') {
            steps {
                bat '"C:/Users/annec/AppData/Local/Programs/Python/Python313/python.exe" -m venv .venv'
                bat '.venv\\Scripts\\python -m pip install -r requirements.txt'
            }
        }

        stage('Compilacao') {
            steps {
                bat '.venv\\Scripts\\python -m compileall -q conversor.py test_conversor.py'
            }
        }

        stage('Testes e cobertura') {
            steps {
                script {
                    bat 'if not exist reports mkdir reports'

                    catchError(buildResult: 'UNSTABLE', stageResult: 'UNSTABLE') {
                        bat '.venv\\Scripts\\python -m pytest -v --junitxml=reports\\junit.xml --cov=conversor --cov-report=term-missing --cov-report=html'
                    }

                    junit 'reports/junit.xml'
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'htmlcov/**, reports/**', allowEmptyArchive: true
        }
    }
}
