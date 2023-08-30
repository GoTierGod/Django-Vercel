# Deploying a Django App on Vercel: Step-by-Step Guide

In this guide, we will walk you through the process of deploying a Django app on Vercel in a clear and concise manner.

## Setting Up the Django Project

Let's begin by creating a basic Django project within a virtual environment.

1. Create Project Folder: Open your terminal and create a new project folder, then navigate into it:

   ```bash
   mkdir django-vercel
   cd django-vercel
   ```

2. Set Up Virtual Environment: Create a virtual environment using the `virtualenv` module:

   ```bash
   python -m virtualenv venv
   ```

3. Activate the Environment: Activate the virtual environment:

   - On Windows

     ```bash
     .\venv\Scripts\activate
     ```

   - On Linux

     ```bash
     source venv/bin/activate
     ```

4. Install Django: Use `pip` to install Django:

   ```bash
   pip install django
   ```

5. Create Project and App: Create a Django project and a new app within it:

   ```bash
   django-admin startproject myproject .
   python manage.py startapp myapp
   ```

## Configuring Vercel Deployment

Now let's configure the deployment settings for Vercel.

1. Create Configuration Files: Create the necessary configuration files:

   - On Windows

     ```bash
     New-Item vercel.json
     New-Item build_files.sh
     ```

   - On Linux

     ```bash
     touch vercel.json
     touch build_files.sh
     ```

2. Configure `vercel.json`: Open the file and add the following configuration:

   ```bash
   {
      "builds": [
         {
            "src": "myproject/wsgi.py",
            "use": "@vercel/python"
         },
         {
            "src": "build_files.sh",
            "use": "@vercel/static-build",
            "config": {
               "distDir": "staticfiles"
            }
         }
      ],
      "routes": [
         {
            "src": "/static/(.*)",
            "dest": "/static/$1"
         },
         {
            "src": "/(.*)",
            "dest": "myproject/wsgi.py"
         }
      ]
   }
   ```

3. Configure `build_files.sh`: Open the file and add the following commands:

   ```bash
   pip install -r requirements.txt
   python3.9 manage.py collectstatic
   ```

4. Update `settings.py`: On the file which located within the myproject directory and make the following changes:

   ```bash
   STATIC_URL = "static/"
   STATICFILES_DIRS = (os.path.join(BASE_DIR, "static"),)
   STATIC_ROOT = os.path.join(BASE_DIR, "staticfiles")
   ```

5. Create `requirements.txt`: Generate the file containing project
   dependencies:

   ```bash
   pip freeze > requirements.txt
   ```

6. Adjust Database Configuration: As Vercel doesn't support `SQLite`, remove the `DATABASES` configuration:

   ```bash
   DATABASES = {}
   ```

7. Update `wsgi.py`: At the end of the file within the myproject directory, add:

   ```bash
   app = application
   ```

8. Add Vercel Domain to `ALLOWED_HOSTS`: In your project's settings.py, add .vercel.app to `ALLOWED_HOSTS`:

   ```bash
   ALLOWED_HOSTS = ['.vercel.app']
   ```

By following these steps, you'll be able to successfully deploy your Django app on Vercel while maintaining a clean and organized structure.
