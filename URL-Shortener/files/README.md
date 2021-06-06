
 

### [ShortMe](http://shortme.biz/) - Project Link

<!-- TABLE OF CONTENTS -->
## Table of Contents

* [About The Project](#About-The-Project)
    + [Hosted version](#Hosted-version)
    + [How it Works](#How-it-Works)
    + [Built With](#built-with)
    + [Extensions](#Extensions)
* [Run Locally](#Run-Locally)
* [API](#API)
    + [Usage](#Usage)
    + [Generate an Authorization Token](#Generate-an-Authorization-Token)
    + [Shorten a URL](#Shorten-a-URL)
    + [Get total URL clicks](#Get-total-URL-clicks)
* [Architecture](#Architecture)
* [Project Structure](#project-structure)
* [Contributing](#Contributing)
* [License](#license)

<!-- about -->
## About The Project

### ShortMe

A Flask web app and API used to shorten long URLs.

#### Hosted Vesrsion
[http://ShortMe/biz](http://shortme.biz/)

#### How it Works
When submitting a URL, it goes through a validity check and added http:// prefix in case it was not provided.  
Once done, a random base64 string is generated and added to an SQLite database with the submitted corresponding URL.  
The user is then given a short URL formatted like so: WEBSITE_DOMAIN/token, the token being the base 64 string.   
Whenever this unique URL is visited, the user will get redirected automatically to the corresponding URL in the database.

![screenshot](https://user-images.githubusercontent.com/10364402/102009696-ea6b4800-3d41-11eb-9444-851515552cb6.png)

#### Built With
* [Flask](https://flask.palletsprojects.com/en/1.1.x/) - Backend
* [Bootstrap](https://getbootstrap.com) - Frontend
* CSS for styling

#### Extensions
* [Flask-RESTful](https://flask-restful.readthedocs.io/en/latest/) - API
* [Flask-SQLalchemy](https://flask-sqlalchemy.palletsprojects.com/en/2.x/) - SQL ORM
* [Flask-Testing](https://flask.palletsprojects.com/en/0.12.x/testing/) - API testing
* [Selenium WebDriver](https://www.selenium.dev/projects/) - UI Automation
* [unittest](https://docs.python.org/3/library/unittest.html) - Testing

## Run Locally
Clone using 

    $ git clone https://github.com/AcrobaticPanicc/ShortMe-URL-Shortener.git 

Create a virtual environment for the project and activate it:

    $ virtualenv venv
    $ source venv/bin/activate

Install the required packages:

    $ pip install -r requirements.txt
    
Set the FLASK_APP environment variable:

    $ export FLASK_APP=run.py

Run the app using:

    $ flask run --host=0.0.0.0 --port=5000

Access the app in browser: http://127.0.0.1:5000/

## API
ShortMe's API lets you interact with ShortMe's URL shortening capabilities over an API.

![screenshot](https://user-images.githubusercontent.com/10364402/102009711-fe16ae80-3d41-11eb-9c31-2cbc253e7d50.png)


### Usage
To use the ShortMe's API, you must provide Authorization token in the header of your request.
Example : <Authorization: Bearer {token}>

### Generate an Authorization Token
To generate an Authorization token, the user must verify his/her email.
Once submitting, the email goes through a simple validity check, and then the database is queried to check if the email already exists in the database.
If the email exists in the database and it is already verified, the user will be redirected to the "your api token" page.
In case the email exists in the database but it is not verified, the user will be redirected to the code verification page to enter the code that was previously sent to his email.
If the email does not exist in the database, the user will be given a verification code to his email and will be redirected to 
the code verification page in order to enter the verification code and activate his email to receive an API token.

![screenshot](https://user-images.githubusercontent.com/10364402/102009247-c5290a80-3d3e-11eb-80b0-b1751f7154e2.png)

![screenshot](https://user-images.githubusercontent.com/10364402/102009249-c8bc9180-3d3e-11eb-8d93-186673ec9024.png)

![screenshot](https://user-images.githubusercontent.com/10364402/102009250-c9edbe80-3d3e-11eb-97ad-b4dfed36c006.png)
 

### Shorten a URL
Given a full URL, returns a ShortMe short URL. 
Currently, the API only supports shortening a single URL per API call.

* URL: `/api/shorten`
* Method: `POST`
* Header Params: Json containing the authorization token `{"Authorization": "Bearer 946bde8d85fc86c6c48344adf1ef2139"}`
* Request Params: Json containing the long URL `{"url": "www.longurl.com/long"}`
* Response: Returns short url data on success

Example call:
    POST: http://shortme.biz/shorten?url=http://www.longurl.com

Example Response:
```
{
    'short_url': shortme.biz/f3Jds, 
    'original_url': 'http://www.longurl.com', 
    'success':True
}
```

### Get total URL clicks
Given a short URL, returns the number of times it has been clicked.  
Currently, the API only supports shortening a single URL per API call.

* URL: `/api/total_clicks`
* Method: `GET`
* Request Params: Json containing the short URL `{"url": "shortme.biz/f3Jds"}`
* Response: Returns total short url visit count

Example call:

    GET http://shortme.biz/api/total_clicks?url=shortme.biz/f3Jds

Example Response:

```
{
    'total': 2,
    'short_url': 'shortme.biz/f3Jds',
    'original_url': 'http://www.longurl.com',
    'success': True
}
```


## Architecture
* The framework selected is Flask because of the ease of its setup and the code readability.
* The Database selected is SQLite and it is implemented with Flask-SQLAlchemy extensions.
* The API was built with the Flask-RESTful extension for Flask.  

## Project Structure
```
.
- LICENSE
- Procfile
- README.md
- app
|   - api
|   |   - __init__.py
|   |   - api.py
|   |   - api_auth.py
|   - app.py
|   - db
|   |   - __init__.py
|   |   - extensions.py
|   |   - models.py
|   - settings.py
|   - setup.py
|   - static
|   |   - CSS
|   |   |   - styles.css
|   |   - JS
|   |   |   - main.js
|   |   - favicon.ico
|   - templates
|   |   - base.html
|   - tests
|   |   - api_testing
|   |   |   - __init__.py
|   |   |   - api_helper.py
|   |   |   - settings.py
|   |   |   - test.sqlite3
|   |   |   - test_api.py
|   |   - front_end_testing
|   |   |   - SeleniumUtility.log
|   |   |   - __init__.py
|   |   |   - index
|   |   |   |   - __init__.py
|   |   |   |   - index.py
|   |   |   - result
|   |   |   |   - __init__.py
|   |   |   |   - result.py
|   |   |   - settings.py
|   |   |   - test_front_end.py
|   |   |   - total_clicks
|   |   |   |   - __init__.py
|   |   |   |   - total_clicks.py
|   |   - utilities
|   |   |   - __init__.py
|   |   |   - logger.py
|   |   |   - selenium_utility.py
|   - views
|   |   - __init__.py
|   |   - api_doc
|   |   |   - __init__.py
|   |   |   - api_doc.py
|   |   |   - templates
|   |   |       - api_doc.html
|   |   - error
|   |   |   - __init__.py
|   |   |   - error.py
|   |   |   - templates
|   |   |       - error.html
|   |   - get_token
|   |   |   - __init__.py
|   |   |   - get_token.py
|   |   |   - templates
|   |   |       - get_token.html
|   |   - index
|   |   |   - __init__.py
|   |   |   - index.py
|   |   |   - templates
|   |   |       - index.html
|   |   - internal
|   |   |   - __init__.py
|   |   |   - favicon.py
|   |   |   - redirect_to_url.py
|   |   |   - send_verification_code.py
|   |   |   - shorten_url.py
|   |   - page_not_found
|   |   |   - __init__.py
|   |   |   - page_not_found.py
|   |   |   - templates
|   |   |       - 404.html
|   |   - total_clicks
|   |   |   - __init__.py
|   |   |   - templates
|   |   |   |   - total-clicks.html
|   |   |   - total_clicks.py
|   |   - verify_code
|   |   |   - __init__.py
|   |   |   - templates
|   |   |   |   - verify.html
|   |   |   - verify_code.py
|   |   - your_api_token
|   |   |   - __init__.py
|   |   |   - templates
|   |   |   |   - your_api_token.html
|   |   |   - your_api_token.py
|   |   - your_short_url
|   |   |   - __init__.py
|   |   |   - templates
|   |   |   |   - your_short_url.html
|   |   |   - your_short_url.py
- requirements.txt
- run.py
```

<!-- CONTRIBUTING -->
## Contributing

Contributions are what makes the open source community such an amazing place to learn, inspire, and create. 
Any contributions you make are **greatly appreciated**.

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

<!-- LICENSE -->
## License

Distributed under the MIT License. See `LICENSE` for more information.
