## Back-End Application Development Capstone – Pictures Microservice

This repository is one of the final parts of the Back‑End Application Development Capstone Project. In this phase we document and ship the Pictures microservice.

This Flask service exposes a REST API to serve pictures from past concerts stored in `backend/data/pictures.json`, including health, simple metrics, and full CRUD operations for the `picture` resource.

## Project structure

- `app.py`: service entrypoint (port 8080)
- `backend/__init__.py`: Flask app initialization
- `backend/routes.py`: endpoints (health, count, picture CRUD)
- `backend/data/pictures.json`: sample data for pictures
- `tests/`: API tests with `pytest`
- `Dockerfile`: container image definition

## Endpoints

Default local base URL: `http://localhost:8080`

- Health
  - `GET /health` → `{"status": "OK"}`

- Simple metrics
  - `GET /count` → `{"length": <int>}` total number of pictures loaded

- Pictures
  - `GET /picture` → list all pictures
  - `GET /picture/<id>` → get a single picture by `id` (404 if not found)
  - `POST /picture` → create a picture (JSON body). Returns 302 if `id` already exists.
  - `PUT /picture/<id>` → replace the picture with that `id` (404 if not found)
  - `DELETE /picture/<id>` → delete the picture (204 if deleted, 404 if not found)

Quick examples (cURL):

```bash
curl -s http://localhost:8080/health
curl -s http://localhost:8080/count
curl -s http://localhost:8080/picture
curl -s http://localhost:8080/picture/1
curl -s -X POST http://localhost:8080/picture \
  -H 'Content-Type: application/json' \
  -d '{"id": 999, "title": "New Pic", "url": "https://example.com/pic.jpg"}'
```

## Requirements

- Python 3.10+
- `pip`; a virtual environment is recommended

Install dependencies:

```bash
python -m venv .venv
source .venv/bin/activate  # Windows PowerShell: .venv\\Scripts\\Activate.ps1
pip install -r requirements.txt
```

## Run locally

```bash
python app.py
# Service listens on http://0.0.0.0:8080
```

## Tests

```bash
pytest -q
```

## Docker

Build the image and run the container:

```bash
docker build -t pictures-svc:latest .
docker run --rm -p 8080:8080 pictures-svc:latest
```

## Notes

- Data is kept in memory at runtime; mutations (POST/PUT/DELETE) are not persisted to file.
- This microservice is consumed by the broader Capstone (alongside the Songs service with MongoDB and the main Django application).
