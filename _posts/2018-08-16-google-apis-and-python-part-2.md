---
title: 'Google APIs and Python – Part II'
date: 2018-08-16
permalink: /posts/2018/08/google-apis-and-python-part-2/
tags:
  - Python
  - Google APIs
  - Google Sheets
  - Google Slides
  - Google Drive
---

Google services are cool and you can build products and services around it. We will see through examples how you can use various google services such as spreadsheet, slides and drive through Python. I hope people can take ideas from the following example to do amazing stuffs with Google services. There is a part one to this article where I walked through procedure to enable Google APIs, installation of required packages in Python, authentication and demonstrated individual examples of Sheets, Drive and Slides API. https://www.thetaranights.com/brief-introduction-to-google-apissheets-slides-drive/ . In this article however, we will integrate Sheets, Drive and Slides API altogether.

## The Idea
We will use data from a sheet which contains some statistics about a few applications/websites. The end goal is to create a presentation slide, add a background image to it, add content from the sheet to the slide and also some other cool stuffs. All the resources used in the following examples are public so you can follow along.

I will be using following resources throughout the example

Presentation Template https://docs.google.com/presentation/d/1wLimfuGw1pqZZJvkc15lOfD7LSEAWCjuIlhbrOaiulE/edit?usp=sharing
Background Images
https://drive.google.com/file/d/0B54qUrMD2GDIa2syZWF3OE5xSUk/view?usp=sharing
https://drive.google.com/file/d/1riNC6PykmjmokXa1My3KyseoKO8MrZGI/view?usp=sharing
https://drive.google.com/file/d/10RUMiifQ7XQn88kSaDy2G83U5sjTeyBS/view?usp=sharing
https://drive.google.com/file/d/1hc8UOmxbFzAS7IaASRZwOftnBBRfBjIs/view?usp=sharing
Spreadsheet https://docs.google.com/spreadsheets/d/1xpjQkF692lNnTsfOckVll2OPTa659ZCuK3JezDSkris/edit?usp=sharing

## Create Presentation Slides from Sheets data and Drive images using Python

```python
from googleapiclient import discovery
from httplib2 import Http
from oauth2client import file, client, tools

TEMPLATE_FILE = "TEM_F"

SCOPES = ('https://www.googleapis.com/auth/spreadsheets','https://www.googleapis.com/auth/drive')

CLIENT_SECRET = 'client_secret_760822340075-i0ark1h51pnbhii5dgafug3k4g1nodb8.apps.googleusercontent.com.json' # download from google console after activating apis

store = file.Storage('storage.json') # doesn't matter if not present, you will be prompted to accept access to google resources on your account and a token will be generated that is stored inside storage.json with requested previliges.

credz = store.get()

if not credz or credz.invalid:
    flow = client.flow_from_clientsecrets(CLIENT_SECRET, SCOPES)
    credz = tools.run_flow(flow, store)

HTTP = credz.authorize(Http())

SHEETS = discovery.build('sheets', 'v4', http=HTTP)

SLIDES = discovery.build('slides', 'v1', http=HTTP)

DRIVE = discovery.build('drive', 'v3', http=HTTP)


presentation_template_file_id = "1wLimfuGw1pqZZJvkc15lOfD7LSEAWCjuIlhbrOaiulE" # the template has been made public.
# name of the presentation file
DATA = {'name':'MobileApplicationsReport'}

PRESENTATION_ID = DRIVE.files().copy(body=DATA, fileId="1wLimfuGw1pqZZJvkc15lOfD7LSEAWCjuIlhbrOaiulE").execute()['id']
print(PRESENTATION_ID)

sheet_ID = '1xpjQkF692lNnTsfOckVll2OPTa659ZCuK3JezDSkris' # the sheet where we fetch data from to populate to the slides.

application_statistics = SHEETS.spreadsheets().values().get(range='Sheet1', spreadsheetId=sheet_ID).execute().get('values') # all the data from the sheet as lists including headers.

print(application_statistics)

presentation_details = SLIDES.presentations().get(presentationId=PRESENTATION_ID).execute()

slides_data = presentation_details.get('slides', [])[0]

page_id = slides_data['objectId'] # page id of the first slide of the presentation.

for each_data in application_statistics[1:]: # skip the headers.
    # duplicate slide for the next cycle before replacing content on a slide since we are using method of replacing text from the slide to populate data.
    reqs = [{"duplicateObject": {"objectId": page_id}}]
    copy_slide_rsp = SLIDES.presentations().batchUpdate(body={'requests':reqs}, presentationId=PRESENTATION_ID).execute()
    
    IMG_ID = each_data[10] # the id of the image present on google drive which we intend to have as a background image to this particular slide.
    img_url = '%s&access;_token=%s' % (DRIVE.files().get_media(fileId=IMG_ID).uri, credz.access_token)
    print("Image url", img_url)

    # prepare a bulk requests that basically replaces the text from the template with the actual data from the sheets.
    bulk_requests = [
        {'updatePageProperties':{'objectId':page_id, 'pageProperties':{'pageBackgroundFill':{'stretchedPictureFill':{'contentUrl':img_url}}}, 'fields':'pageBackgroundFill'}},
        {'replaceAllText':{'containsText':{'text':'{{SHOWCASE  NAME}}', 'matchCase':True}, 'replaceText':each_data[1], "pageObjectIds": [page_id]}},
        {'replaceAllText':{'containsText':{'text':'{{DESCRIPTION}}', 'matchCase':True}, 'replaceText':each_data[2], "pageObjectIds": [page_id]}},
        {'replaceAllText':{'containsText':{'text':'{{COMPOSITION}}', 'matchCase':True}, 'replaceText':each_data[3], "pageObjectIds": [page_id]}},
        {'replaceAllText':{'containsText':{'text':'{{IMPRESSIONS}}', 'matchCase':True}, 'replaceText':each_data[8], "pageObjectIds": [page_id]}},
        {'replaceAllText':{'containsText':{'text':'{{VIDEO VIEWS}}', 'matchCase':True}, 'replaceText':each_data[7], "pageObjectIds": [page_id]}},
        {'replaceAllText':{'containsText':{'text':'{{USERS}}', 'matchCase':True}, 'replaceText':each_data[6], "pageObjectIds": [page_id]}},
        {'replaceAllText':{'containsText':{'text':'{{MOBILE}}', 'matchCase':True}, 'replaceText':each_data[9], "pageObjectIds": [page_id]}}
    ]
    bulk_update_response = SLIDES.presentations().batchUpdate(body={'requests':bulk_requests}, presentationId=PRESENTATION_ID, fields='').execute().get('replies')

    page_id = copy_slide_rsp['replies'][0]['duplicateObject']['objectId'] # update the page id as the one that was duplicated so we now can work on this slide.

delete_final_page = SLIDES.presentations().batchUpdate(body={'requests':[{"deleteObject": {"objectId": page_id}}]}, presentationId=PRESENTATION_ID, fields='').execute().get('replies')
```

## Output Presentation Created:
After successful running of the above program, following presentation was generated.
https://docs.google.com/presentation/d/1h9YqUnCWu5pxXmW3rs_9rKmMsVeJM9I8nIBGbr25pME/edit?usp=sharing