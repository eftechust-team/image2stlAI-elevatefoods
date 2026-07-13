# AI Image to STL Generator

Generate multi-layer STL files from AI-generated black-and-white images with lasso/wand selection and adjustable edge dilation.

## Features

- AI image generation via Doubao API.
- Per-browser image quota capped at 4 generated images.
- Canvas editing (pen, bucket, undo/redo).
- Selection tools: Magic Wand and Lasso with add/subtract.
- Expand Selection (px) to close gaps without cumulative growth.
- Per-layer height and stacking options; STL export as a ZIP.

## Local Run

1. Install dependencies:
	```bash
	pip install -r requirements.txt
	```
2. Set the environment variable for the Doubao API key (optional locally):
	```bash
	$env:DOUBAO_API_KEY="<your-key>"   # PowerShell
	```
3. Start the app:
	```bash
	python app.py
	```
4. Open http://localhost:8080

## Deploy on Render

This repo includes `render.yaml` for one-click deploy.

- Build: `pip install -r requirements.txt`
- Start: `gunicorn app:app`
- Environment variables:
  - `DOUBAO_API_KEY`: your Doubao API key (set in Render Dashboard → Environment)
	- `FLASK_SECRET_KEY`: stable secret used for per-session image quotas
  - `PYTHON_VERSION`: 3.11 (provided in render.yaml)
	- `GOOGLE_DRIVE_UPLOAD_ENABLED`: set to `true` to upload generated files automatically
	- `GOOGLE_DRIVE_FOLDER_ID`: target Google Drive folder ID, defaults to the folder you provided
	- `GOOGLE_DRIVE_SERVICE_ACCOUNT_JSON`: service account JSON string, or
	- `GOOGLE_DRIVE_SERVICE_ACCOUNT_FILE`: path to a service account JSON file

Important:

- Share the target Google Drive folder with the service account email before deploying.
- If the service account cannot access the folder, the STL/ZIP download still works, but the Drive upload will fail and the error will be shown in the status message.

Optional tuning:

- `MAX_IMAGES_PER_SESSION`: default `4`
- `MAX_CONCURRENT_REQUESTS`: default `2`
- `DOUBAO_REQUEST_TIMEOUT`: default `90`

Steps:
- Create a new Web Service from this repo.
- Render will detect `render.yaml` and configure the service.
- Set `DOUBAO_API_KEY` in the service’s Environment tab (as a Secret).
- Deploy. Auto-deploy is enabled.

## Notes

- The Doubao API key must be provided via `DOUBAO_API_KEY`. If missing, image generation returns a clear error.
- Logo at `/static/logo.jpeg` is optional and can be added later.