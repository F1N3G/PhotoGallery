# PhotoGallery

A web photo manager built with Flask and SQLite. Upload images, organise them
into albums with tags, and browse them in a gallery view.

## Features

- Upload with extension whitelisting (`.jpg`, `.jpeg`, `.png`, `.webp`, `.gif`)
- Album and tag organisation
- Gallery view ordered by upload date
- Delete removes both the database record and the file on disk
- Collision-free storage: files are renamed with a microsecond timestamp,
  while the original filename is kept in the database

## Tech Stack

| Layer | Technology |
|---|---|
| Backend | Python, Flask |
| Database | SQLite |
| Templates | Jinja2 |

## Data Model

```sql
CREATE TABLE photos (
    id            INTEGER PRIMARY KEY AUTOINCREMENT,
    filename      TEXT NOT NULL,   -- stored name (timestamped)
    original_name TEXT NOT NULL,   -- name as uploaded
    title         TEXT NOT NULL,
    album         TEXT NOT NULL DEFAULT 'General',
    tags          TEXT NOT NULL DEFAULT '',
    size_bytes    INTEGER NOT NULL,
    uploaded_at   TEXT NOT NULL
);
```

Indexed on `uploaded_at` and `album`, the two columns the gallery filters and
sorts by.

## Routes

| Method | Path | Purpose |
|---|---|---|
| GET | `/gallery` | Gallery view |
| GET | `/upload` | Upload form |
| POST | `/upload` | Handle upload |
| POST | `/photo/<id>/delete` | Delete photo and file |
| GET | `/uploads/<filename>` | Serve stored image |

## Getting Started

```bash
git clone https://github.com/F1N3G/PhotoGallery.git
cd PhotoGallery

python -m venv .venv
source .venv/bin/activate      # Windows: .venv\Scripts\activate

pip install flask
python app.py
```

Runs at `http://localhost:5000`. The database and `uploads/` directory are
created automatically on first run.

## Possible Extensions

- User accounts, so albums are per-user
- Thumbnail generation instead of serving full-size images in the gallery
- Pagination for large collections
