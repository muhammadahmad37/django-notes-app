
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/muhammadahmad37/django-notes-app.git'
            }
        }

        stage('Create Virtual Environment') {
            steps {
                sh 'python3 -m venv django'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'django/bin/python -m pip install --upgrade pip'
                sh 'django/bin/python -m pip install -r requirements.txt'
                sh 'django/bin/python -m pip install setuptools==80.9.0'
            }
        }

        stage('Run Django') {
            steps {
                sh 'django/bin/python manage.py makemigrations'
                sh 'django/bin/python manage.py migrate'

                sh '''
                    pkill -f "gunicorn.*notesapp.wsgi" || true

                    nohup django/bin/gunicorn notesapp.wsgi:application \
                    --bind 0.0.0.0:8000 \
                    --workers 2 \
                    > django.log 2>&1 &
                '''
            }
        }
    }
}
