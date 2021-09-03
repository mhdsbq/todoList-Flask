to do app

https://www.youtube.com/watch?v=yKHJsLUENl0

Aim:
    1 learn handiling different routs in flask
    2 using a database
    3 stylising

structure:
    1 a title
    2 an add to do text field 
    3 add button
    4 list of already added to dos
    5 ability to update status of a todo and delete  a todo

build:
    1 enable a virtual environmant
    2 activate virtual environment 
    3 import flask and app = Flask(__name__)
    4 use @app.route to route a fuction
    5 return the page html from the function decorated by app.route("route")
    6 to run flask.run() , debug = True is an optional arguement
    7 to return html we use render template module from flask
    8 template should be in /templates directory
    9 add a forum to add a to do task in template html file
        - forum
            - label -todo title
            - input -placeholder add todo
            - br tag -- as a brake
            - button -type "submit"
    10 pip install sql alchamy for sql functons
    11 set config variables
        - app.config['SQLALCHEMY_DATABASE-URI'] = 'sqlite:///db.sqlite'
        - app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
    12 create database
        - db = SQLAlchemy(app)
    13 create a class which handle these database datas
        - class Todo holds 3 datas
            1 id -Integer 
            2 task -string
            3 status -Boolean
        - class is inherited from -db.Model
    14 code to add data to a database
        - new_todo = Todo(title="todo1", compleate=False)
        - db.session.add(new_todo)
        - db.session.commit()
    15 code to quirey database
        - todo_list = Todo.query.all()
    16 now to acces this query in index page, pass this todo_list variable to render template and render template
        can handle this usin jinja2 synthax
            - render_template("index.html", todo_list=todo_list )
    17 write jinja 2 syntax to print all the to do task and with its id
        - {% for todo in todo_list %}
                "we can acces each todo iterations here"
                eg: <p> {{ todo.id }} - {{ todo.title }} </p>
          {% endfor %}
    18 showing the status of task
        - if in jinja2
            {% if todo.complete==False %}
                "if thing" -- use a span to show status
            {% else %}
                "else thing"
            {% endif %}
    19 update and delete button
        <a href="/">Update</a>
        <a href="/">Delete</a> "we have to give database operatins to these links"
    20 lets get into functionalities
        (I) a function to add new task
            - first we have to check the forum to get the title of todo
            - we use request module of Flsak to do this
            - import this from flask
            - title = request.form.get("name of the forum") - here name of the forum is title
            - add function

                def add():
                    title = request.form.get("title")
                    new_todo = Todo(title=title, completed=False)
                    db.session.add(new_todo)
                    db.session.commit()

                # now we have to refresh website with new data
                # for that we use 2 modules redirect and url_for

                    return redirect(url_for("/refresh path"))
                
                # we have to set a route for this operation by specifing the method
                # here we set up in path "/add" and method=["POST"]

            - now set form action and form method in html
                <form action="/add" method="POST">
        
        (II) A function to update status

            - we have to pass the Todo id to the backend
                - this can be done by setting up the app route to "/update/<int:todo_id>"
            - after getting the id, backend function will change the database value and refresh the page with return redirect(url_for) thingy..:)

            @app.route("/update/<int:todo_id>")
            def update(todo_id):
                todo = Todo.query.filter_by(id=todo_id).first()
                todo.complete = not todo.complete
                db.session.commit()
                return redirect(url_for("index"))

            - href this route in html
                <a href="/update/{{ todo.id }}">Update</a>
            

        (III) Delete a task
            - todo id is passed to backend
            - this is deletd and commited from db 
            - page is refreshed
            
            @app.route("/delete/<int:todo_id>")
            def delete(todo_id):
                todo = Todo.query.filter_by(id=todo_id).first()
                db.session.delete(todo)
                db.session.commit()
                return redirect(url_for("index"))
                
            - href this route in html
                <a href="/delete/{{ todo.id }}">Delete</a>
    21 make it clean using semantic ui
        - https://semantic-ui.com/
        - take the cdn links and paste it to the head of the html page

            <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/semantic-ui@2.4.2/dist/semantic.min.css">
            <script src="https://cdn.jsdelivr.net/npm/semantic-ui@2.4.2/dist/semantic.min.js"></script>