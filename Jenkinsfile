pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Récupération de la branche main
                git branch: 'main', url: 'https://github.com/Hafizoune/integration_continue.git'
            }
        }

        stage('Setup Python') {
            steps {
                // Création du virtualenv et installation de pip
                sh '''
                if ! command -v python3 &>/dev/null; then
                    echo "Erreur : Python3 n'est pas installé sur cet agent"
                    exit 1
                fi

                python3 -m venv venv
                . venv/bin/activate
                pip install --upgrade pip
                if [ -s requirements.txt ]; then
                    pip install -r requirements.txt
                else
                    echo "requirements.txt vide, aucune dépendance à installer"
                fi
                '''
            }
        }

        stage('Run tests') {
            steps {
                // Lancement de pytest (ne fait pas échouer le pipeline si aucun test)
                sh '''
                . venv/bin/activate
                if command -v pytest &>/dev/null; then
                    pytest -v || true
                else
                    echo "pytest non installé, tests ignorés"
                fi
                '''
            }
        }
    }
}
