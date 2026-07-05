# Django + Celery Integration: Step-by-Step Guide

This guide will walk you through integrating Celery into a Django project from scratch. You do not need to reference any other guides—every step, code snippet, and explanation is included here.

---

## 1. Install Dependencies

In your project root, activate your virtual environment and install Celery and Redis (as the message broker):

```bash
pip install celery redis
```

Install Redis server on your system (Linux/WSL):
```bash
sudo apt install redis-server
```

---

## 2. Create the Celery App

In your Django project folder (where `settings.py` is), create a file called `celery.py`:

```python
# celery.py
import os
from celery import Celery

os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'your_project.settings')  # Replace 'your_project' with your project name

app = Celery('your_project')
app.config_from_object('django.conf:settings', namespace='CELERY')
app.autodiscover_tasks()
```

---

## 3. Update `__init__.py` to Load Celery

In the same folder as `settings.py`, edit (or create) `__init__.py`:

```python
from .celery import app as celery_app

__all__ = ('celery_app',)
```

---

## 4. Add Celery Settings to `settings.py`

At the end of your `settings.py`, add:

```python
# Celery configuration
CELERY_BROKER_URL = 'redis://localhost:6379/0'
CELERY_RESULT_BACKEND = 'redis://localhost:6379/0'
CELERY_ACCEPT_CONTENT = ['json']
CELERY_TASK_SERIALIZER = 'json'
CELERY_RESULT_SERIALIZER = 'json'
```

---

## 5. Create a Sample Task

In any app (e.g., `myapp`), create a file called `tasks.py`:

```python
from celery import shared_task

@shared_task
def add(x, y):
    return x + y
```

---

## 6. Start Redis Server

If installed via apt, Redis runs as a service. Check status:
```bash
sudo systemctl status redis-server
```
If not running, start it:
```bash
sudo systemctl start redis-server
```

---

## 7. Run Celery Worker

From your project root:
```bash
celery -A your_project worker --loglevel=info
```
Or create a helper script (e.g., `run_celery.sh`):
```bash
#!/bin/bash
source .venv/bin/activate
exec celery -A your_project worker --loglevel=info
```
Then:
```bash
chmod +x run_celery.sh
./run_celery.sh
```

---

## 8. Test Your Setup

In the Django shell:
```bash
python manage.py shell
>>> from myapp.tasks import add
>>> add.delay(2, 3)
```
You should see the result in the Celery worker's output.

---

## 9. Notes
- Replace `your_project` with your Django project name everywhere.
- Replace `myapp` with your Django app name for tasks.
- For production, use systemd or supervisor to manage Celery and Django processes.

---

**You now have Celery fully integrated with Django and Redis as a broker!**
