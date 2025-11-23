---
title: 'Introduction to Google APIs with Python'
date: 2018-08-12
permalink: /posts/2018/08/google-apis-python/
tags:
  - Python
  - Google APIs
  - Google Sheets
  - Google Slides
  - Google Drive
---

The intentions of this post is to familiarize usage of Google APIs with Python. Google services are cool and you can build products and services around it. We will see through examples how you can use various google services such as spreadsheet, slides and drive through Python. I hope people can take ideas from the following example to do amazing stuffs with Google services. In order to work with google services via their APIs, first we need to create a project on google console with specific APIs enabled. For the scope of this article, we will need the SHEETS API, SLIDES API and DRIVE API enabled.

I will be using following resources throughout the examples

Presentation Template https://docs.google.com/presentation/d/1wLimfuGw1pqZZJvkc15lOfD7LSEAWCjuIlhbrOaiulE/edit?usp=sharing
Background Image https://drive.google.com/file/d/0B54qUrMD2GDIa2syZWF3OE5xSUk/view?usp=sharing
Spreadsheet https://docs.google.com/spreadsheets/d/1xpjQkF692lNnTsfOckVll2OPTa659ZCuK3JezDSkris/edit?usp=sharing

## Installation of libraries and setup

```
pip install --upgrade google-api-python-client oauth2client
```

## Creating a project on Google Console and enabling APIs

1. Open google console https://console.cloud.google.com/apis/dashboard
2. Create a new project

create_a_project_google_console
Create a new project on google console

3. Name the project

new_project_google_console
Name new project

4. Enable Sheets, Slides and Drive APIs

google_console_enable_apis
Enable Google APIs

google_console_enable_drive_api
Enable Drive API as well as slides and sheets APIs

5. Create Credentials and Download it.

google_console_create_credentials
Create credentials for the project and download it

## Authentication

We need the credentials that was downloaded from google console for authentication. Google creates an access token to access and work on the google resources. The token does expire and in case it does, we will be prompted on a browser to provide access to the application for the specified resources on our google account.

```python
>>> from googleapiclient import discovery
>>> from httplib2 import Http
>>> from oauth2client import file, client, tools
>>> SCOPES = ('https://www.googleapis.com/auth/spreadsheets','https://www.googleapis.com/auth/drive')
>>> CLIENT_SECRET = 'client_secret_760822340075-i0ark1h51pnbhii5dgafug3k4g1nodb8.apps.googleusercontent.com.json'
>>> store = file.Storage('token.json')
>>> creds = store.get()
/home/bhishan-1504/googleapis/googleapienv/lib/python3.6/site-packages/oauth2client/_helpers.py:255: UserWarning: Cannot access token.json: No such file or directory
  warnings.warn(_MISSING_FILE_MESSAGE.format(filename))
>>> 

>>> if not credz or credz.invalid:
...     flow = client.flow_from_clientsecrets(CLIENT_SECRET, SCOPES)
...     credz = tools.run_flow(flow, store)
...

Your browser has been opened to visit:

    https://accounts.google.com/o/oauth2/auth?client_id=760822340075-i0ark1h51pnbhii5dgafug3k4g1nodb8.apps.googleusercontent.com&redirect;_uri=http%3A%2F%2Flocalhost%3A8080%2F&scope;=https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fspreadsheets+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fdrive&access;_type=offline&response;_type=code

If your browser is on a different machine then exit and re-run this
application with the command-line parameter

  --noauth_local_webserver

Created new window in existing browser session.
Authentication successful.
>>>
```

Note: For all of the examples below, we need authentication. I am skipping them in all of the examples underneath to remove redundancy.

## Reading a google spreadsheet using Python

```python
>>> HTTP = credz.authorize(Http())
>>> SHEETS = discovery.build('sheets', 'v4', http=HTTP)
>>> sheet_ID = '1xpjQkF692lNnTsfOckVll2OPTa659ZCuK3JezDSkris'
>>> spreadsheet_read = SHEETS.spreadsheets().values().get(range='Sheet1', spreadsheetId=sheet_ID).execute()
>>> spreadsheet_read
{'range': 'Sheet1!A1:Z1001', 'majorDimension': 'ROWS', 'values': [['Category', 'Showcase Name', 'Description', 'Audience Composition', 'ID', 'Audience Name', 'Users', 'Video Views', 'Impressions', 'Mobile Impressions', 'Image'], ['Sports', 'Sports Fans', 'People who likes sports', 'online data', '12', 'Sports Fans', '1000', '1000', '1000', '1000', 'sports-fans.jpg']]}
>>> spreadsheet_values = spreadsheet_read['values']
>>> spreadsheet_values
[['Category', 'Showcase Name', 'Description', 'Audience Composition', 'ID', 'Audience Name', 'Users', 'Video Views', 'Impressions', 'Mobile Impressions', 'Image'], ['Sports', 'Sports Fans', 'People who likes sports', 'online data', '12', 'Sports Fans', '1000', '1000', '1000', '1000', 'sports-fans.jpg']]
>>>
```

## Search and Download a file from drive using Python

```python
>>> import io
>>> DRIVE = discovery.build('drive', 'v3', http=HTTP)
>>> resp = DRIVE.files().list(q="name='{filepath}'".format(filepath="P1150264.JPG")).execute()
>>> resp
{'kind': 'drive#fileList', 'incompleteSearch': False, 'files': [{'kind': 'drive#file', 'id': '0B54qUrMD2GDIa0FMdkxLMmpoZVU', 'name': 'P1150264.JPG', 'mimeType': 'image/jpeg'}]}
>>> file_id = resp['files'][0]['id']
>>> file_id
'0B54qUrMD2GDIa0FMdkxLMmpoZVU'
>>> file_request = DRIVE.files().get_media(fileId=file_id)
>>> fh = io.BytesIO()
>>> downloader = MediaIoBaseDownload(fh, file_request)
>>> done = False
>>> while done is False:
...     status, done = downloader.next_chunk()
...     print("Download {status}".format(status=status.progress() * 100))
...
Download 100.0
>>>
```

## Google Slides API

### Create a blank presentation using Python

```python
>>> SLIDES = discovery.build('slides', 'v1', http=HTTP)
>>> body = {'title': 'AutomatedPresentation'}
>>> presentation_request = SLIDES.presentations().create(body=body).execute()
>>> presentation_request['presentationId']
'1ROJOeVFaA4PbC2voR5EFddohxQlZvUkrdi1dsJUks9c'
>>>
```

Follow the link to see the presentation the above code snippet creates.
https://docs.google.com/presentation/d/1ROJOeVFaA4PbC2voR5EFddohxQlZvUkrdi1dsJUks9c/edit?usp=sharing

### Creating presentation using existing template from drive

```python
>>>TEMPLATE_FILE = 'TEM_F'
>>> SLIDES = discovery.build('slides', 'v1', http=HTTP)
>>> DRIVE = discovery.build('drive', 'v3', http=HTTP)
>>> rsp = DRIVE.files().list(q="name='%s'"% TEMPLATE_FILE).execute()['files'][0]
>>> rsp
{'kind': 'drive#file', 'id': '1wLimfuGw1pqZZJvkc15lOfD7LSEAWCjuIlhbrOaiulE', 'name': TEMPLATE_FILE, 'mimeType': 'application/vnd.google-apps.presentation'}
>>> DATA = {'name': 'PresentationUsingTemplate'}
>>> create_presentation_request = DRIVE.files().copy(body=DATA, fileId=rsp['id']).execute()
>>> presentation_id = create_presentation_request['id']
>>> presentation_id
'10iDjayeyVkVSp5F6eQIqzpISAqjFlbqG4_jdYDAFJG4'
>>>
```

Follow the link to see the presentation the above code snippet creates. https://docs.google.com/presentation/d/10iDjayeyVkVSp5F6eQIqzpISAqjFlbqG4_jdYDAFJG4/edit?usp=sharing

### Adding background image to a slide

```python
>>> SLIDES = discovery.build('slides', 'v1', http=HTTP)
>>> DRIVE = discovery.build('drive', 'v3', http=HTTP)
>>> rsp = DRIVE.files().list(q="name='%s'"% TEMPLATE_FILE).execute()['files'][0]
>>> rsp
{'kind': 'drive#file', 'id': '1wLimfuGw1pqZZJvkc15lOfD7LSEAWCjuIlhbrOaiulE', 'name': 'TEM_F', 'mimeType': 'application/vnd.google-apps.presentation'}
>>> DATA = {'name': 'PresentationUsingTemplatePlusBackgroundImage'}
>>> create_presentation_request = DRIVE.files().copy(body=DATA, fileId=rsp['id']).execute()
>>> presentation_id = create_presentation_request['id']
>>> presentation_id
'1cxpaH19h582Q4Ot3b5GL9U6ETl9myqE3JlX4_Fa35e8'
>>> IMG_FILE = "sports-fans.jpg"
>>> img_file_request = DRIVE.files().list(q="name='%s'" % IMG_FILE).execute()['files'][0]
>>> img_url = '%s&access_token=%s' % (DRIVE.files().get_media(fileId=img_file_request['id']).uri, credz.access_token)
>>> img_url
'https://www.googleapis.com/drive/v3/files/0B54qUrMD2GDIa2syZWF3OE5xSUk?alt=media&access_token=ya29.Glz3BcRtfadsfGzKwUQ-6llroeaMfdasfdaffadsjfdXiOewDdHqhdgBef2euMm9OMxGXyXF-axwZ0gFBwH2-T6qS29qmpc-H3ELcyh7CDZCbfzn7DTNJkugoA'

>>> presentation_details = SLIDES.presentations().get(presentationId=presentation_id).execute()
>>> first_slide_data = presentation_details.get('slides', [])[0]
>>> first_slide_id = slides_data['objectId']
>>> first_slide_id
'p3'
>>> bulk_reqs = [{'updatePageProperties':{'objectId':first_slide_id, 'pageProperties':{'pageBackgroundFill':{'stretchedPictureFill':{'contentUrl':img_url}}}, 'fields':'pageBackgroundFill'}}]
>>> bulk_update_req = SLIDES.presentations().batchUpdate(body={'requests':bulk_reqs}, presentationId=presentation_id).execute()
```

Follow the link to see the presentation the above code snippet creates.
https://docs.google.com/presentation/d/1cxpaH19h582Q4Ot3b5GL9U6ETl9myqE3JlX4_Fa35e8/edit?usp=sharing

On a follow up post to this one, we will focus on integrating slides, sheets and drive API altogether. We shall use spreadsheet data and populate it onto presentation slides.