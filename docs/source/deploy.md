# Voila — Deployment docs
The deployment docs are split up in two parts.
First there is the general section, which should always be followed.
Then there is a cloud service provider specific section of which one should be chosen.

# Setup an example project
1. Create a project directory of notebooks you wish to display. For this tutorial we will clone voila and treat the notebooks folder as our project root.
```bash
$ git clone git@github.com:QuantStack/voila.git
$ cd voila/notebooks/
```
2. Add a requirements.txt file to the project directory. For this tutorial we will copy the contents of the environment.yml of voila. Omit xleaflet and xeus-cling because these require extra work that is beyond the scope of this guide.
```txt
ipyvolume
bqplot
scipy
ipympl
voila==0.1.2
ipympl
# xleaflet==0.7.0 
# xeus-cling==0.5.1
```

# Cloud Service Providers (pick one)
## Deployment on Heroku
Heroku.com is an attractive option if you want to try out deployment for free. You have limited computing hours, however the app will also automatically shutdown if it is idle.

The general steps for deployment at Heroku can be found [here](https://devcenter.heroku.com/articles/getting-started-with-python).
High level instructions, specific to voila can be found below:

1. Follow the steps of the official documentation to install the heroku cli and login on your machine.
2. Add a file named runtime.txt to the project directory:
```txt
python-3.7.3
```
4. Add a file named Procfile to the project directory:
With the following line if you want to show all notebooks:
```
web: voila —-port=$PORT --no-browser
```
Or the following if you only want to show one notebook
```
web: voila —-port=$PORT —-no-browser your_notebook.ipynb
```
5. Initialize your git repo and commit your code
```bash
$ git init .
$ git add .
$ git commit -m "my message"
```
6. Create an Heroku instance and push the code.
```bash
$ heroku create
$ git push heroku master
``` 
7. Open your web app
```bash
$ heroku open
```

## Deployment on Google App Engine
You can deploy on [Google App Engine](https://cloud.google.com/appengine/) in a “flexible” environment. This means that the underlying machine will always run. This is more expensive than a “standard” environment, which is similar to Heroku’s free option. However, Google App Engine’s “standard” environment does not support websockets, which is a requirement for voila.

The general steps for deployment at Google App Engine can be found [here](https://cloud.google.com/appengine/docs/flexible/python/quickstart).
High level instructions specific to voila can be found below:

1. Follow the “Before you begin steps” from the official documentation to create your: 1)account, 2) project and 3) App Engine app.
2. Add an app.yaml file to the project directory:
```yaml
runtime: python
env: flex
runtime_config:
    python_version: 3
entrypoint: voila --port=$PORT --no-browser 
```
3. Edit the last line if you want to show only one notebook
```yaml
entrypoint: voila --port=$PORT --no-browser your_notebook.ipynb
```
4. Deploy your app
```bash
$ gcloud app deploy
```
5. Open your app
```bash
$ gcloud app browse
```

