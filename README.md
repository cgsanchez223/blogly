- Chapter 27.1 SQLAlchemy
    - SQLAlchemy is an ORM (object-relational-mapper) - tools used to create and manipulate models of database tables through a programming language
    - Can be used to write Python code that executes in a Postgres database
____________________________________________________________________________
- To install first you have to be familiar with installing Flask.
- Once you are in the virtual environment you can install SQLAlchemy with the finally lines in your Terminal shell
    - $ pip install psycopg2-binary
    - $ pip install flask-sqlalchemy
- Example for an OO model
    - class Pet(db.Model):
    - """Pet."""

    - __tablename__ = "pets"

    - id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    - name = db.Column(db.String(50), nullable=False, unique=True)
    - species = db.Column(db.String(30), nullable=True)
    - hunger = db.Column(db.Integer, nullable=False, default=20)

    ___________________________________________________________
    - In SQL, the above code would look like
        - CREATE TABLE pets ( id SERIAL PRIMARY KEY, name VARCHAR(50) NOT NULL UNIQUE, species VARCHAR(30), hunger INTEGER NOT NULL DEFAULT 20)

_______________________________________________________________
- Setup:
    - models.py:
        - from flask_sqlalchemy import SQLAlchemy
        - db = SQLAlchemy()
        - def connect_db(app):
            - """Connect to database."""
            - db.app = app
            - db.init_app(app)

    - app.py:
        - from flask import Flask, request, redirect, render_template
        - from models import db, connect_db, Pet
        - app = Flask(__name__)
        - app.config['SQLALCHEMY_DATABASE_URI'] = 'postgresql:///sqla_intro'
        - app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
        - app.config['SQLALCHEMY_ECHO'] = True
        - connect_db(app)

- Explanation:
    - SQLALCHEMY_DATABASE_URI = Where is your database?
    - SQLALCHEMY_TRACK_MODIFICATIONS - Provides info on modifications if set to true. False cleans up readability
    - SQLALCHEMY_ECHO - Print all SQL statements to the terminal

- Creating the Database:
    - $ ipython3
        - % run app.py
        - db.create_all()

_________________________________________________________________________
- Assignment:
- Blogly
    - Blogly is a site where you can create a user profile. Users will be added to a database. A user's information can be saved, updated, and deleted.
    - Create a User Model with the following columns: id, first_name, last_name, image_url
    - Available routes created:
        - GET / - Redirect to list of users
        - GET /users - Shows all users
        - GET /users/new - Shows an add for for new users
        - POST /users/new - Posts the add form, redirects back to /users
        - GET /users/[user-id] - Shows information about the given user. Has an edit button.
        - GET /users/[user-id]/edit - Can edit a user. Can cancel to redirect to user page or save button to update user.
        - POST /users/[user-id]/edit - Process information, returns to /users page
        - POST /users/[user-id]/delete - Can delete the user