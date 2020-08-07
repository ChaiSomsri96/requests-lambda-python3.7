# requests-lambda-python3.7
Python requests module from AWS Lambda(Python 3.7)

> How to use this

``
import requests
import json
# from extract import extract_element_from_json
# from formatter import formatter

class PDF_Response:
  def __init__(self, status_code, file_64):
    self.status_code = status_code
    self.file_64 = file_64

def pdf_create(data):
    ret = object()
    response = requests.post(
        url="*****************************",
        headers={
            "X-Auth-Key": "*****************",
            "X-Auth-Secret": "*******************",
            "Content-Type": "application/json; charset=utf-8",
            "Accept": "application/json",
        },
        data = data
    )
    parsed_json = json.loads(response.content.decode('utf8'))
    referraldocument = parsed_json['response']
    file_64 = referraldocument
    #file_64 = formatter(referraldocument)
    file_64 = str(file_64)
    file_64 = file_64.replace('\\n','')
    file_64 = file_64.replace('\'\\','')
    file_64 = file_64.replace(file_64[:2], '')
    file_64 = file_64[:-2]
    ret = PDF_Response(response.status_code, file_64)
    return ret


def send_request():
    # Request
    # GET https://eu-api.jotform.com/submission/**********

    try:
        response = requests.get(
            url="https://eu-api.jotform.com/submission/**********",
            params={
                "apiKey": "*****************",
            },
            headers={
                "Cookie": "******************",
            },
        )
        #return ('Response HTTP Status Code: {status_code}'.format(
        #    status_code=response.status_code))
        return response
    except requests.exceptions.RequestException:
        return ('HTTP Request failed')

def lambda_handler(event, context):
    responseGet = send_request()
    if responseGet.status_code == 200 :
        responsePost = pdf_create(responseGet.content)
    return {
        'statusCode': 200,
        'responseGetStatus': responseGet.status_code,
        'responsePostStatus': responsePost.status_code,
        'PDF Generator B64': responsePost.file_64,
        'body': "Success"
    }
``
