# Guide: Python Flask API CRUD practice

## Create a new Python Flask app

1.  Start with a clean slate

    - Close all your text editor windows, quit your text editor.
    - Find and kill any terminal servers using Ctrl C.
    - Open Terminal and navigate to your Actualize folder.

2.  In your terminal, create a new folder:

    <pre><code>mkdir <ins><strong>name-of-your-app</strong></ins></code></pre>

3.  In the terminal, go to your new folder

    <pre><code>cd <ins><strong>name-of-your-app</strong></ins></code></pre>

4.  In the terminal, enter the command to make a Python virtual environment:

    ```
    python3 -m venv .venv
    ```

5.  In the terminal, enter the command to activate your virtual environment:

    ```
    source .venv/bin/activate
    ```

6.  In the terminal, enter the command to install Flask:

    ```
    pip install Flask
    ```

7.  In the terminal, enter the command to open your text editor:

    ```
    code .
    ```

8.  In the text editor, make a new file called `.gitignore` and paste in the following code:

    ```
    .venv/

    *.pyc
    __pycache__/

    instance/

    .pytest_cache/
    .coverage
    htmlcov/

    dist/
    build/
    *.egg-info/
    ```

9.  In the text editor, make a new file called `app.py` and paste the following code:

    ```
    from flask import Flask, request

    app = Flask(__name__)


    @app.route('/')
    def hello():
        return 'Hello, World!'
    ```

10. In the terminal, start your server by entering:

    ```
    flask --app app run --debug
    ```

11. In your browser, visit http://127.0.0.1:5000 to make sure your app is working

> In your terminal, open a new tab by entering the shortcut Command T, then use git to save a version of your code.
>
> ```bash
> git init
> git add --all
> git commit -m "Initial commit"
> ```

## Create the data layer

1.  In your text editor, make a new file called `db.py` and paste the following code:

    ```
    import sqlite3


    def connect_to_db():
        conn = sqlite3.connect("database.db")
        conn.row_factory = sqlite3.Row
        return conn


    def initial_setup():
        conn = connect_to_db()
        conn.execute(
            """
            DROP TABLE IF EXISTS photos;
            """
        )
        conn.execute(
            """
            CREATE TABLE photos (
              id INTEGER PRIMARY KEY NOT NULL,
              name TEXT,
              width INTEGER,
              height INTEGER
            );
            """
        )
        conn.commit()
        print("Table created successfully")

        photos_seed_data = [
            ("1st photo", 800, 400),
            ("2nd photo", 1024, 768),
            ("3rd photo", 200, 150),
        ]
        conn.executemany(
            """
            INSERT INTO photos (name, width, height)
            VALUES (?,?,?)
            """,
            photos_seed_data,
        )
        conn.commit()
        print("Seed data created successfully")

        conn.close()


    if __name__ == "__main__":
        initial_setup()
    ```

2.  In your terminal, run the following command to create your database:

    ```
    python3 db.py
    ```

    _(this should create a new file called `database.db`)_

    > In your terminal, use git to save a version of your code.
    >
    > ```bash
    > git add --all
    > git commit -m "Add database with tables"
    > ```

## Create the web layer

1. In `app.py`, import the database

   ```python
   import db
   ```

2. Make the **INDEX** action

   - <details><summary>In <code>db.py</code>, define a function to get all photos</summary>

     ```python
     def photos_all():
         conn = connect_to_db()
         rows = conn.execute(
             """
             SELECT * FROM photos
             """
         ).fetchall()
         return [dict(row) for row in rows]
     ```

     </details>

   - <details><summary>In <code>app.py</code>, add a route</summary>

     ```python
     @app.route("/photos.json")
     def index():
         return db.photos_all()
     ```

     </details>

   - Use a tool like [curl](https://curl.se/docs/manpage.html), [Postman](https://www.postman.com/), or [HTTPie](https://httpie.io/product/) to make a GET request to <code>localhost:5000/<ins><strong>photos</strong></ins>.json</code> and see the response

     > In your terminal, use git to save a version of your code.
     >
     > ```bash
     > git add --all
     > git commit -m "Add index action"
     > ```

3. Make the **CREATE** action

   - <details><summary>In <code>db.py</code>, define a function to create a photo</summary>

     ```python
     def photos_create(name, width, height):
         conn = connect_to_db()
         row = conn.execute(
             """
             INSERT INTO photos (name, width, height)
             VALUES (?, ?, ?)
             RETURNING *
             """,
             (name, width, height),
         ).fetchone()
         conn.commit()
         return dict(row)
     ```

     </details>

   - <details><summary>In <code>app.py</code>, add a route</summary>

     ```python
     @app.route("/photos.json", methods=["POST"])
     def create():
         name = request.form.get("name")
         width = request.form.get("width")
         height = request.form.get("height")
         return db.photos_create(name, width, height)
     ```

     </details>

   - Use a tool like [curl](https://curl.se/docs/manpage.html), [Postman](https://www.postman.com/), or [HTTPie](https://httpie.io/product/) to make a POST request to <code>localhost:5000/<ins><strong>photos</strong></ins>.json</code> with body parameters and see the response

     > In your terminal, use git to save a version of your code.
     >
     > ```bash
     > git add --all
     > git commit -m "Add create action"
     > ```

4. Make the **SHOW** action

   - <details><summary>In <code>db.py</code>, define a function to find a photo by id</summary>

     ```python
     def photos_find_by_id(id):
         conn = connect_to_db()
         row = conn.execute(
             """
             SELECT * FROM photos
             WHERE id = ?
             """,
             id,
         ).fetchone()
         return dict(row)
     ```

     </details>

   - <details><summary>In <code>app.py</code>, add a route</summary>

     ```python
     @app.route("/photos/<id>.json")
     def show(id):
         return db.photos_find_by_id(id)
     ```

     </details>

   - Use a tool like [curl](https://curl.se/docs/manpage.html), [Postman](https://www.postman.com/), or [HTTPie](https://httpie.io/product/) to make a GET request to <code>localhost:5000/<ins><strong>photos</strong></ins>/:id.json</code> and see the response

     > In your terminal, use git to save a version of your code.
     >
     > ```bash
     > git add --all
     > git commit -m "Add show action"
     > ```

5. Make the **UPDATE** action

   - <details><summary>In <code>db.py</code>, define a function to update a photo</summary>

     ```python
     def photos_update_by_id(id, name, width, height):
         conn = connect_to_db()
         row = conn.execute(
             """
             UPDATE photos SET name = ?, width = ?, height = ?
             WHERE id = ?
             RETURNING *
             """,
             (name, width, height, id),
         ).fetchone()
         conn.commit()
         return dict(row)
     ```

     </details>

   - <details><summary>In <code>app.py</code>, add a route</summary>

     ```python
     @app.route("/photos/<id>.json", methods=["PATCH"])
     def update(id):
         name = request.form.get("name")
         width = request.form.get("width")
         height = request.form.get("height")
         return db.photos_update_by_id(id, name, width, height)
     ```

     </details>

   - Use a tool like [curl](https://curl.se/docs/manpage.html), [Postman](https://www.postman.com/), or [HTTPie](https://httpie.io/product/) to make a PATCH request to <code>localhost:5000/<ins><strong>photos</strong></ins>/:id.json</code> with body parameters and see the response

     > In your terminal, use git to save a version of your code.
     >
     > ```bash
     > git add --all
     > git commit -m "Add update action"
     > ```

6. Make the **DESTROY** action

   - <details><summary>In <code>db.py</code>, define a function to destroy a photo</summary>

     ```python
     def photos_destroy_by_id(id):
         conn = connect_to_db()
         row = conn.execute(
             """
             DELETE from photos
             WHERE id = ?
             """,
             id,
         )
         conn.commit()
         return {"message": "Photo destroyed successfully"}
     ```

     </details>

   - <details><summary>In <code>app.py</code>, add a route</summary>

     ```python
     @app.route("/photos/<id>.json", methods=["DELETE"])
     def destroy(id):
         return db.photos_destroy_by_id(id)
     ```

     </details>

   - Use a tool like [curl](https://curl.se/docs/manpage.html), [Postman](https://www.postman.com/), or [HTTPie](https://httpie.io/product/) to make a DELETE request to <code>localhost:5000/<ins><strong>photos</strong></ins>/:id.json</code> and see the response

     > In your terminal, use git to save a version of your code.
     >
     > ```bash
     > git add --all
     > git commit -m "Add destroy action"
     > ```

## Next steps

- Configure CORS to allow frontend requests: https://flask-cors.readthedocs.io/en/latest/
- Explore other ways to organize code in directories: https://flask.palletsprojects.com/en/3.0.x/tutorial/
- Explore an ORM (Object Relational Mapping) to organize database code in models https://flask-sqlalchemy.palletsprojects.com/en/3.1.x/quickstart/
