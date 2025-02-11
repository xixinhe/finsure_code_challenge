{{TOC}}

## Language version  
Python 3.8.8

## To start up the api application:

1. checkout repository from github  
```
git clone https://github.com/xixinhe/finsure.git
```  
  
2. change DB config to your db configuration in below section in __PROJECT_ROOT/finsure_code_challenge/finsure_code_challenge/settings.py__  
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'lenders',
        'USER': 'root',
        'PASSWORD': 'root',
        'HOST': '127.0.0.1',
        'PORT': '3306',
    }
}
```  
  
3. make sure there's a database __lenders__ on MariaDB, and user above has permission to modify it.
  
4. MariaDB connection requires __mysqlclient__ python lib to be installed. Before installing that, make sure you have installed the mysql client.  
  
5. go to __PROJECT_ROOT__, create virtual environment  
```
python -m venv .venv
```  

6. install dependencies  
```
pip install -r requirements.txt
```   

7. go to __PROJECT_ROOT/finsure_code_challenge__, setup database structure  
```
python manage.py makemigrations
python manage.py migrate
```  

8. start up application  
```
python manage.py runserver
```  

9. browse to localhost:8000/lenders/  
  
## API Description:

### - GET /lenders/
**Description:** List all Lenders (five per page), filter by active or inactive status  
  
**Request:**
- parameters:  
  - page_number: integer  
    for pagination purpose, starting from 1  
  - active: boolean (true/false)  
    for filtering lender by active/inactive status  
  
**Response:**  
  - 200, lenders information is returned without issue
  
### - POST /lenders/
**Description:** Create a new Lender  
  
**Request:**  
- format: application/vnd.api+json  
- body:  
```
{
    "data": {
        "type": "Lender",
        "id": null,
        "attributes": {
            "name": "Jason",
            "code": "Adams",
            "upfront_commission_rate": 0.45,
            "trial_commission_rate": 0.54,
            "active": true
        }
    }
}
```  
  
**Response:**   
  - 201, lender is created without issue  
  - 400, user request is bad or json is not in correct format  
  
### - GET /lenders/{id}/  
**Description:** Get a specific Lender  
  
**Response:**  
  - 200, specific lender information is returned  
  - 404, not able to find the lender  
  
### - PUT /lenders/{id}/  
**Description:** Update a specific Lender
  
**Request:**  
- format: application/vnd.api+json  
- body:  
```
{
    "data": {
        "type": "Lender",
        "id": "118",
        "attributes": {
            "name": "Richard Johnson updated",
            "code": "rjo",
            "upfront_commission_rate": 0.4,
            "trial_commission_rate": 0.34,
            "active": true
        }
    }
}
``` 
  
**Response:**  
  - 200, lender information is successfully updated  
  - 400, user request is bad, incorrect user input  
  - 409, user input id conflicts with url's lookup id  
  
### - DELETE /lenders/{id}/  
**Description:** Delete a specific Lender  
  
**Response:**  
  - 204, completed, no further action is required  
  
### - GET /lenders/csv/  
**Description:** Download Lenders in CSV format  
  
**Response:**  
  - 200, csv file containing all lender information will start  
  - 405, HTTP method not allowed

### - POST /lenders/upload/  
**Description:** Bulk upload Lenders in CSV format  
  
**Request:**  
- format: form_data  
- body: file upload from multipart html form  
  
**Response:**  
  - 201, lenders information are created
  - 400, user request is bad, either no file is uploaded or csv format is wrong
  - 405, HTTP method is not allowed
