# Flask Blog Platform

---

## Project Summary

A full-stack blog platform built with Python and Flask. The application supports user registration and authentication, multi-user blog post creation and management, and a threaded comment system. Role-based access control restricts administrative actions (creating, editing, and deleting posts) to a designated admin account. The front end uses Bootstrap 5 and CKEditor for a clean, responsive writing experience. The app is backed by SQLAlchemy ORM and is deployable to both Vercel and traditional WSGI hosts via Gunicorn.

---

### Tech Stack

| Layer            | Technology                             |
| ---------------- | -------------------------------------- |
| Language         | Python 3                               |
| Web Framework    | Flask 2.3                              |
| ORM              | SQLAlchemy 2 / Flask-SQLAlchemy        |
| Auth             | Flask-Login, Werkzeug password hashing |
| Forms            | Flask-WTF, WTForms                     |
| Rich Text Editor | Flask-CKEditor                         |
| UI               | Bootstrap 5 (Bootstrap-Flask)          |
| Avatars          | Flask-Gravatar                         |
| Database (dev)   | SQLite                                 |
| Database (prod)  | PostgreSQL (psycopg2-binary)           |
| WSGI Server      | Gunicorn                               |
| Deployment       | Vercel                                 |

---

### Features

- **User authentication** — register, log in, and log out with securely hashed and salted passwords
- **Admin-only controls** — the first registered user (id = 1) automatically receives admin privileges; only admins can create, edit, or delete posts
- **Blog post management** — full CRUD via a rich-text CKEditor interface
- **Comment system** — authenticated users can comment on any post; comments are linked to both the author and the parent post via foreign-key relationships
- **Gravatar integration** — user profile images are pulled automatically from Gravatar based on email
- **Responsive UI** — Bootstrap 5 layout with custom CSS overrides
- **Environment-based config** — secrets and database URIs are loaded from `.env` to keep credentials out of source control

---

### Project Structure

```
blog_site/
├── main.py              # App factory, DB models, and all route handlers
├── forms.py             # WTForms form definitions
├── requirements.txt     # Python dependencies
├── Procfile             # Gunicorn entry point for WSGI hosts
├── vercel.json          # Vercel deployment configuration
├── static/
│   ├── css/styles.css   # Custom styles
│   └── js/scripts.js    # Custom scripts
└── templates/
    ├── header.html      # Shared navigation header
    ├── footer.html      # Shared footer
    ├── index.html       # Post listing (home page)
    ├── post.html        # Individual post + comments
    ├── make-post.html   # Create / edit post form
    ├── register.html    # Registration page
    ├── login.html       # Login page
    ├── about.html       # About page
    └── contact.html     # Contact page
```

---

### Database Models

```
User ──< BlogPost   (one user authors many posts)
User ──< Comment    (one user writes many comments)
BlogPost ──< Comment (one post has many comments)
```

---

### Getting Started

#### 1. Clone the repository

```bash
git clone https://github.com/<your-username>/flask-blog-platform.git
cd flask-blog-platform
```

#### 2. Create and activate a virtual environment

```bash
python3 -m venv venv
source venv/bin/activate
```

#### 3. Install dependencies

```bash
pip install -r requirements.txt
```

#### 4. Configure environment variables

Create a `.env` file in the project root:

```env
SECRET_KEY=your_secret_key_here
DATABASE_URL=sqlite:///posts.db   # or your PostgreSQL connection string
```

#### 5. Run the development server

```bash
flask --app main run --debug
```

The app will be available at `http://127.0.0.1:5000`.

---

### Admin Account

The **first user to register** is automatically treated as the administrator. To protect admin routes, the app uses an `@admin_only` decorator that returns a `403 Forbidden` error if the current user's id is not `1`.

---

### License

MIT
