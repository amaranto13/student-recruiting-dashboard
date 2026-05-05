# Student Recruiting Support Dashboard

A Flask-based job application tracker for students.

## Tech Stack
- Python 3.11
- Flask 3.x + Flask-SQLAlchemy 3.x
- Bootstrap 5.3 (CDN)
- SQLite (local dev) — zero-config swap to PostgreSQL for production

## Local Development

```bash
pip install -r requirements.txt
python app.py
```

App runs at http://localhost:5000

## Environment Variables

| Variable | Default | Description |
|---|---|---|
| `DATABASE_URL` | `sqlite:///applications.db` | Full SQLAlchemy DSN. Set to a PostgreSQL URL for production (see below). |
| `PORT` | `5000` | Port the Flask server listens on. Azure App Service sets this automatically. |
| `SECRET_KEY` | `dev-secret-...` | Flask session secret — **always set a strong value in production**. |

## Deployment to Azure App Service

1. Create an Azure PostgreSQL Flexible Server and note the connection string.
2. In your Azure App Service → Configuration → Application Settings, set:
   - `DATABASE_URL` = `postgresql+psycopg2://user:pass@hostname/dbname`
   - `SECRET_KEY` = (a long random string)
   - `PORT` is set automatically by Azure
3. Tables are created automatically on first run via `db.create_all()`.
4. The `Procfile` is used by Azure for startup: `gunicorn --bind 0.0.0.0:$PORT app:app`

## Deploying to GitHub

```bash
git init
echo "applications.db" >> .gitignore
echo "__pycache__/" >> .gitignore
git add .
git commit -m "Initial commit"
git remote add origin <your-repo-url>
git push -u origin main
```

The database file (`applications.db`) should not be committed to Git.
For production, use `DATABASE_URL` pointing to a managed PostgreSQL instance.
