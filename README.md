Author: A. Nihiser | apnihiser@gmail.com | https://github.com/merkkie/

DISCLAIMER: This is still very rough and insecure, do not run in any production environments!!!

VISION: To create a script that will allow CDR reporting against a Cisco Call Manager version 10.5.2.12901-1 (tested against).

Current Functionality: Updated the original script into a full blown application that includes the following modules.
  1) SQLite3 Database
  2) Flask Framework
  3) Flask-SQLalchemy interaction between web GUI and database
  4) Basic Forms to collect report paramenters.
  5) Flask-Security
  6) Python general scripting to hold it all together.

Current Status: On Hold for now. I think I have accomplished all I have set out to do. HTML may not be the most solid ever but there is a much better results table now along with the ability to export your results to excel. I have created the homepage as well as a proper login page. I will be monitoring any comments and questions if they pop up. I trying to redouble my efforts into learning cleaner code and test based development so there will not be many noticible gains on this project until I go back over it with my knowledge but I am starting a new project that will help with catagorizing and taking inventory of UCCX audio prompts. I am hoping to have a script that goes through all the wav files and transcribes them into text. I think I have found the best library for this so please say in touch for more develpments. As I said please contact me for any assistance or questions into this project.

Any comments or questions about this project please reach out to me and I'll see what I can do to implement or answer any questions.

Installation: 

  1) Decide how your going to host your application. I'm going to try to provide instructions for hosting on both apache and IIS, so I'll get back to you on that. For the meantime if you need to get this up and going there are many instructions available for getting flask hosted in all sorts of places.

  2) Create a Virtual Environment if needed

  3) Install Modules, I installed the following:
    a) Flask
    b) flask-SQLalchemy
    c) Flask-Migrate
    d) flask-Security
  4) Config your config.py file.
    a) You can change the name of the DB
    b) Set your SECRET_KEY for CSRF security
    c) Change your login Hash and Salt Parameters.
  5) Create your DB
    a) Set your environmental variables: for powershell $env:FLASK_APP = "certifycdr.py"
    b) After the previous step is completed run flask db init from the \CertifyCDR directory
    c) Run flask db migrate, with this complete you will be able to upgrade and downgrading using flask db migrate
    d) Run flask db upgrade! You should have all your tables filled out now.
    e) For more information on this topic see https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-iv-database
  6) Create a user.
    a) Locate and uncomment  the following in routes.py   
        Create a user to test with
        @app.before_first_request
        def create_user():
            models.db.create_all()
            user_datastore.create_user(name='', password='')
            models.db.session.commit()
    b) Create a name and username      
    c) If you need to add the environmental variables from 4.a and enter the command 'flask run'.
    d) Browse out to http://localhost:5000/login and login using the credents from step 5.b
    e) Go back and delete or comment out the decorator and function from step 5.b or you will get errors.
  7) Import files into database
    a) At this point you should have your Cisco call manager dumping car files into CertifyCDR/app/inserts
    b) You can schedule car files to dump in real-time, hourly, or however you wish, so when we set up import.py keep this in mind.
    c) Schedule your imports file to process files in a manner that makes sense to you. For instance if you wanted to 
    create an hourly import you might do somthing like this:
      1) CAR will dump files at xx:50 of the hour and schedule import.py to process at xx:59:99.
      2) On windows I've found the following settings help on a 2012 server. 
        a) Run whether user is logged on or not
        b) Run with highest priviledges
        c) Start a program, C:\Windows\System32\cmd.exe, add arguments cmd /c python C:\CertifyCDR\app\imports.py
        d) Start in (optional): C:\CertifyCDR\app
        e) One Time trigger to kick it out afterwards 1 hour indefinately.

