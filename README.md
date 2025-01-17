# Sentiment Analysis App 
This application was made as a project to implement end to end Machine Learning. Then goal was to train a sentiment analysis app using scraped data and predict the rating for a review. The app was then deployed to EC2 and accessed as a web app. 

### Running the app :
The app can be run either as a Docker compose or a python web-app.
1. To run with Docker, install [Docker](https://docs.docker.com/engine/install/ubuntu/) and [Docker Compose](https://docs.docker.com/compose/install/). Then run the following commands :
```
docker-compose build
docker-compose up
```
2. To run as a python web-app run the following in order :

  - PostgreSQL : 
Either install postgresql in your system or install a [docker container](https://docs.docker.com/engine/examples/postgresql_service/). Then run the following command :
``` 
docker run --name postgres -e POSTGRES_USER=postgres_user -e POSTGRES_PASSWORD=password123 -e POSTGRES_DB=postgres_db -p 5432:5432 -d postgres 
```
   - API (Flask) :
Run the below command to start Flask :
```
cd src/api/
python app.py
```
  - Dash :
For running Dash front-end run :
```
cd src/dash/
python app.py
```
For both the above methods, the dash app will be accessible at [http://localhost:8050](http://localhost:8050).

### Phases of the project :

#### 1. Data Collection :hammer:: 
The data was collected form [Trustpilot](https://www.trustpilot.com/). This website has a huge collection of reviews on different shops and companies. It also has a rating for each review which was used to train the model. The scraped data was saved in two CSV files, one with links to all companies and the other with the actual rating and reviews. A total of 1.3GB of reviews was scrapped from the website. Scraping was done using Python [Selenium](https://selenium-python.readthedocs.io/) and [Scrapy](https://scrapy.org/).

<img src="https://github.com/ShubhamBhut/SentimentAnalysis/blob/master/.github_readme_assets/trustpilot_capture.PNG" height="400" width="800">

#### 2. Data Cleaning :sweat_drops::
The data was loaded into a dataframe for basic cleaning and manupulation.

#### 3. Training with CNN :robot::
To use CNN in this case, [1D-Convolutions](https://towardsdatascience.com/understanding-1d-and-3d-convolution-neural-network-keras-9d8f76e29610) is used. The CNN will try to predict if a review is good, average or bad. The review data is feeded to the CNN and after 3 to 4 epochs ~70% accuracy was achieved for all three cases. (I did not go any further as I was training on my laptop with an i5-10210U)

<img src="https://github.com/ShubhamBhut/SentimentAnalysis/blob/master/.github_readme_assets/Annotation%202020-07-30%20143806.png" height="300" width="500">

#### 4. Building a Web-App :iphone::
The web app will predict a rating for the review of one of the [Fortune 500](https://fortune.com/fortune500/) companies. The web app will have four components : 
1. Database ([Postgresql](https://www.postgresql.org/))
2. ML Model (Train from CNN)
3. API ([Flask](https://flask.palletsprojects.com/en/1.1.x/)/[Gunicorn](https://gunicorn.org/))
4. Front-end Client ([Dash](https://plotly.com/dash/))

<img src="https://github.com/ShubhamBhut/SentimentAnalysis/blob/master/.github_readme_assets/webservice%20components%20.png" height="320" width="500">

#### 5. Dockerizing the Application :whale:: 
The web app was "dockerised" using docker-compose. Dockerising will help with version issues and will be easier to deploy in a production environment (in this case EC2 instance). The web app was separated into three containers : db, api and dash. I used the versions of the packages that I downloaded while developing this project in docker. Also, Flask was only used for testing and Gunicorn was used in docker as Flask is not for production deployments.

#### 6. Deploying to AWS EC2 💻 :
I used a free-tier EC2 instance for deloying this web app. "Amazon Linux 2" AMI was selected as the instance with t3a.micro. In port selection we need to keep 8050 port open as it will by our Dash app. Inside the instance we install docker and docker-compose. Then we can clone the git repository and build the web app. One thing to be careful about is t3a.micro only has 1 GB RAM and our instance will run out of menory while installing larger packages like ```torch```. We can modify the dockerfile to download ```torch``` separately and then install to avoid this.([Stackoverflow solution](https://stackoverflow.com/questions/52782864/memoryerror-when-attempting-to-create-a-docker-image-with-torch-pytorch))


<img src="https://github.com/ShubhamBhut/SentimentAnalysis/blob/master/.github_readme_assets/final_deployment_digram.png" height="320" width="600">


#### 7. Help Resources :pencil2::
This project started out as a shameless copy of [MarwanDebbiche/post-tuto-deployment](https://github.com/MarwanDebbiche/post-tuto-deployment) repository. I found their [blog](https://medium.com/datadriveninvestor/end-to-end-machine-learning-from-data-collection-to-deployment-ce74f51ca203) and was inspired to make my copy of it. In the process of implementing it, I learned many new technologies like Docker, Flask and Dash. I am still learning and will make a project with my original idea soon. The authors of the repository I followed were inspired by this repository : [ahmedbesbes/character-based-cnn](https://github.com/ahmedbesbes/character-based-cnn).

[[Back to Phases of Project](https://github.com/ShubhamBhut/SentimentAnalysis/blob/master/README.md#phases-of-the-project-)]
