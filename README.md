# CTRL-SHIFT-ELITE

# CTRL-SHIFT-ELITE: Distributed Job Scheduler

**CTRL-SHIFT-ELITE** is a scalable, fault-tolerant job scheduling system built for automating tasks like file execution, notifications, and system operations. Powered by Django, Kafka, Supabase, and Wasabi S3, it efficiently queues and processes one-time and periodic jobs across distributed workers. Born from a hackathon, it’s ready for real-world automation!

## Features
- **Asynchronous Job Queueing**: Kafka ensures decoupled, reliable job distribution.
- **Task Variety**: Executes Python/Bash scripts, sends email/Slack notifications, runs system commands.
- **Fault Tolerance**: Retries failed jobs, requeues tasks from crashed workers.
- **Scalability**: Kafka consumer groups enable parallel job processing.
- **Persistent Storage**: Wasabi S3 stores job results cost-effectively.
- **Real-Time Database**: Supabase (PostgreSQL) tracks jobs and workers.
- **Monitoring**: Optional Kafka UI for job queue visibility.

## Architecture
- **Controller (`controller.py`)**: Schedules jobs, enqueues to Kafka’s `job_topic`, monitors worker health.
- **Workers (`consumer.py`)**: Kafka consumers process jobs, store results in Wasabi, update Supabase.
- **Kafka**: Message broker for durable job queueing and load balancing.
- **Supabase**: PostgreSQL database for job metadata, worker states, and assignments.
- **Wasabi S3**: Cloud storage for job outputs (e.g., processed files).
- **Django**: Backend framework integrating Supabase and managing logic.
- **Docker Compose**: Runs Kafka and Zookeeper for easy setup.


## Prerequisites
- Python 3.8+
- Docker & Docker Compose
- Django (`requirements.txt`)
- Supabase account (Project URL, API Key)
- Wasabi S3 account (access key, secret key, bucket name)
- Kafka client (`kafka-python`)

## Setup Instructions
1. **Clone the Repository**:
   
   git clone https://github.com/neharika-garg-20/CTRL-SHIFT-ELITE.git
   cd CTRL-SHIFT-ELITE

## Install requirements
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt

SUPABASE_URL=<your-supabase-url>
SUPABASE_KEY=<your-supabase-api-key>
WASABI_ACCESS_KEY=<your-access-key>
WASABI_SECRET_KEY=<your-secret-key>
WASABI_BUCKET=schedular
WASABI_ENDPOINT_URL=https://s3.ap-southeast-1.wasabisys.com
DATABASE_URL=postgres://<user>:<password>@<host>:<port>/<dbname>  # From Supabase

EMAIL_USER='user@example.com'
EMAIL_PASS='password123'


DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': '<dbname>',
        'USER': '<user>',
        'PASSWORD': '<password>',
        'HOST': '<host>',
        'PORT': '<port>',
    }
}


## Start Controller & Workers:


docker-compose up -d

## Run migrations

python manage.py migrate
python manage.py runserver


## Copy the below commands into different terminals: 

python controller.py  # Schedules jobs
python worker.py    # Processes jobs (scale with multiple instances)

Why This Project?
Crafted for a hackathon, CTRL-SHIFT-ELITE excels with:

Kafka: Scalable, durable queueing vs. Celery/RabbitMQ.
Supabase: Real-time PostgreSQL, easier than raw Postgres setup.
Wasabi S3: no subscription fees (30 days) vs. AWS S3.
Future Improvements
Kafka multi-partition topics for job type separation.
Supabase real-time subscriptions for live job updates.
Enhanced REST API for job management.