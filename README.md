# Objectives
From reports the time spent per visitor per SL, article and page, let's recommend other articles to users (=visitors)

# What it does
There is a minimal streamlit app (app.py) and fastapi server (server.py) that can be queried from a smartlink (see script.js that should be set as script in argo editor)


# Quick analysis & Processing 

## Inconsistencies
### Durations
Some inconsistencies with durations => like 2 days

For articles, we have time spent per page that contain an article title. It means that if 3 articles have their titles on the same page, they will have same ratings. And if an article is on 7 pages, only the first page will be taken into account.

## About ratings
Between 0 and 5 rating is just a clamped ratio (per page)= nb_sec/6 (so above 30s =>5) 

### Global rating per SL ?
=> Sum of time per page capped at 30 min ?
=> 

# PDF analysis
This is a test to check what an article is and how we could detect it and maybe enhance the recommendations

It's really a first test. See extract.py


# Installation

## Deployed Streamlit link
<https://grfrecommendations-skrkcd5tqmykmjawehmyr6.streamlit.app/>

## Locally

### Create virtual env
python3.9 -m venv venv
source venv/bin/activate

### Install dependencies
pip install -r requirements.txt

### Run 
* app : streamlit run app.py
    * example data :
        * https://nemato-data.fr/public/output.csv (full)
        * https://nemato-data.fr/public/small.csv (extract)
* server: uvicorn server:app --reload
    * for the demo, I used ngrok as a tunnel : then no https is needed, it's provided by ngrok
    * 2 routes:
        * /generate_test is there to build the model out of <https://nemato-data.fr/public/output.csv>. Needs to be called only once
        * /recommendations/{visitor_id} to get recommations for this vistor

### HTTPS
* create keys
openssl req -x509 -newkey rsa:4096 -nodes -out cert.pem -keyout key.pem -days 365
* create https server
uvicorn server:app --ssl-keyfile key.pem --ssl-certfile cert.pem --reload


### Test
I created an example : 
* Editor : https://ui-dev.argoflow.io/home/projects/659579887e5bd900128038bd
* link and vistor Id:https://dev-org-argo.argoflow.io/test.sl?sl=ff8d9e38d19a6c65ae95123fb344a6da&q=174810-51961

If no visitor Id or not found, then Recommendation (top right) appears

Script is script.js to be inserted as script in editor

