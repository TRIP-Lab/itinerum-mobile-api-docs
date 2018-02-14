---
title: Itinerum API Reference

language_tabs:
  - python

toc_footers:
  - <a href='http://itinerum.ca/'>Itinerum Project</a>

includes:
  - errors

search: true
---

# Introduction

__Welcome to the Itinerum API!__ This documentation describes getting started with the mobile API endpoints for authentication, retrieving survey questions, mode prompts, and uploading user data.

There are 3 types of requests an application will make:
1. Initial request where user account is created and survey schema is sent to device
2. Second POST to server where demographic survey responses are submitted
3. All subsequent server calls to add new GPS points, prompt responses and cancelled prompts

Python code examples are located in the dark area to the right.


# Create a New User

```python
import json
import requests

url = API_URL + '/create'

test_data = {
    "lang": "en",
    "survey_name": "test",
    "user": {
        "itinerum_version": "12c",
        "uuid": "7077a34e-afe5-4c22-bd96-119256b7dc51",
        "os_version": "80.23",
        "model": "iPhone 4s",
        "os": "ios"
    }
}
parameters = json.dumps(test_data)
response = requests.post(url, data=parameters)
```


> The above command returns JSON structured like this:

```json
{
    "lang": "en",
    "termsOfService": "terms of service text",
    "aboutText": "information about survey instance text",
    "contactEmail": "contact@email.com",
    "prompt": {
        "maxDays": 14,
        "prompts": [
            {
                "colName": "dropdown_prompt",
                "prompt": "dropdown prompt title",
                "id": 1,
                "choices": [
                    "Choice 1",
                    "Choice 2"
                ]
            },
            {
                "colName": "checkbox_prompt",
                "prompt": "checkbox prompt title",
                "id": 2,
                "choices": [
                    "Choice 1",
                    "Choice 2"
                ]
            }
        ],
        "maxPrompts": 20,
        "numPrompts": 0
    },
    "uuid": "43f8cdaf-3e43-429d-beb2-94e8220a8a06",
    "defaultAvatar": "/assets/static/defaultAvatar.png",
    "surveyName": "tester",
    "surveyId": 22,
    "survey": [
        {
            "colName": "member_type",
            "prompt": "What is your primary occupation?",
            "id": 104,
            "fields": {
                "choices": [
                    "0",
                    "1",
                    "2",
                    "3",
                    "4",
                    "5"
                ]
            }
        },
        {
            "colName": "travel_mode_work",
            "prompt": "How do you typically commute to your work location?",
            "id": 110,
            "fields": {
                "choices": [
                    "0",
                    "1",
                    "2",
                    "3",
                    "4",
                    "5",
                    "6"
                ]
            }
        },
        {
            "colName": "travel_mode_alt_work",
            "prompt": "Do you use any alternative mode of travel to work?",
            "id": 111,
            "fields": {
                "choices": [
                    "0",
                    "1",
                    "2",
                    "3",
                    "4",
                    "5",
                    "6",
                    "7"
                ]
            }
        },
        {
            "colName": "travel_mode_study",
            "prompt": "How do you typically commute to your studies?",
            "id": 108,
            "fields": {
                "choices": [
                    "0",
                    "1",
                    "2",
                    "3",
                    "4",
                    "5",
                    "6"
                ]
            }
        },
        {
            "colName": "travel_mode_alt_study",
            "prompt": "Do you use any alternative mode of travel to your studies?",
            "id": 109,
            "fields": {
                "choices": [
                    "0",
                    "1",
                    "2",
                    "3",
                    "4",
                    "5",
                    "6",
                    "7"
                ]
            }
        },
        {
            "colName": "Gender",
            "prompt": "What is your gender?",
            "id": 100,
            "fields": {
                "choices": [
                    "0",
                    "1",
                    "2"
                ]
            }
        },
        {
            "colName": "Age",
            "prompt": "What is your age bracket?",
            "id": 101,
            "fields": {
                "choices": [
                    "0",
                    "1",
                    "2",
                    "3",
                    "4",
                    "5"
                ]
            }
        },
        {
            "colName": "dropdown",
            "prompt": "dropdown selection title",
            "id": 1,
            "fields": {
                "choices": [
                    "Choice 1",
                    "Choice 2"
                ]
            }
        },
        {
            "colName": "checkbox",
            "prompt": "checkbox selection title",
            "id": 2,
            "fields": {
                "choices": [
                    "Choice 1",
                    "Choice 2"
                ]
            }
        },
        {
            "colName": "number",
            "prompt": "number selection title",
            "id": 3,
            "fields": {
                "choices": []
            }
        },
        {
            "colName": "text",
            "prompt": "text field input title",
            "id": 5,
            "fields": {
                "choices": []
            }
        },
        {
            "colName": "Email",
            "prompt": "Please enter your email",
            "id": 103,
            "fields": {
                "choices": []
            }
        },
        {
            "colName": "location_work",
            "prompt": "Please enter your work location on the map",
            "id": 107,
            "fields": {
                "latitude": null,
                "longitude": null
            }
        },
        {
            "colName": "address",
            "prompt": "address input title",
            "id": 4,
            "fields": {
                "latitude": null,
                "longitude": null
            }
        },
        {
            "colName": "location_study",
            "prompt": "Please enter your study location on the map",
            "id": 106,
            "fields": {
                "latitude": null,
                "longitude": null
            }
        },
        {
            "colName": "location_home",
            "prompt": "Please enter your home location on the map",
            "id": 105,
            "fields": {
                "latitude": null,
                "longitude": null
            }
        }
    ],
    "user": "New user successfully registered.",
    "avatar": null
}
```

A new user is created within a survey by supplying a randomly generated UUID to identify the user. This needs to be cached within the app as it is required for future requests. The `survey_name` corresponds to the name used to signup the survey via the dashboard. The `avatar` path provides the URI to locate the survey's avatar icon when joined with the root URL. In the example above, the full path would then be: "http://<api.root.url>/assets/static/avatars/93jf3.jpg".

### HTTP Request

`POST http://<api.root.url>/mobile/v1/create`

### Query Parameters

| Parameter   | Description                              |
| ----------- | ---------------------------------------- |
| user        | Dictionary containing `uuid`, `model`, `itinerum_version`, `os`, `os_version` |
| survey_name | Name of survey to associate the user     |

<aside class="notice">
Remember to cache the <code>uuid</code> within the client for future requests.
</aside>

## Parsing the Survey

Each prompt from the returned survey (above) is associated with a field type to map to the response input box. This is returned as `id` within the _create user_ response. The IDs 1-10 correspond to generic input fields, 98-99 are specialty dialogs, and 100-103 are prompts that cannot be deleted on the frontend and will appear in every survey.

| id   | field names         | description  |
| ---- | ------------------- | ------------ |
| 1    | choices             | select one   |
| 2    | choices             | select many  |
| 3    | number              | number input |
| 4    | latitude, longitude | map/address  |
| 5    | text                | text box     |

| id   | __hardcoded fields__    | description                             |
| ---- | ----------------------- | --------------------------------------- |
| 100  | Gender                  | gender                                  |
| 101  | Age                     | age                                     |
| 102  | mode                    | primary mode (unuse  d)                 |
| 103  | Email                   | email                                   |
| 104  | member_type             | primary occupartion                     |
| 105  | location_home           | mark user's home location on map        |
| 106  | location_study          | mark user's study location on map       |
| 107  | location_work           | mark user's work location on map        |
| 108  | travel\_mode_study      | primary travel mode to study location   |
| 109  | travel\_mode\_alt_study | secondary travel mode to study location |
| 110  | travel\_mode_work       | primary travel mode to work location    |
| 111  | travel\_mode\_alt_work  | secondary travel mode to work location  |


# Updating the Database
## Answering the Survey

```python
import requests
import json

test_data = {
    "survey": {
        "checkbox": [
            "Choice 1"
        ],
        "dropdown": "Choice 2",
        "Gender": "1",
        "travel_mode_work": "5",
        "address": {
            "latitude": "-12.5665545",
            "longitude": "-41.789760"
        },
        "number": 5789,
        "travel_mode_alt_work": "7",
        "Email": "vpollich@tillman-heathcote.com",
        "travel_mode_alt_study": "0",
        "member_type": "0",
        "text": "Cumque nemo occaecati aliquid nam iusto rem itaque. Aperiam saepe itaque quas odit accusantium. Recusandae maiores consequatur esse veniam sequi odit.",
        "location_home": {
            "latitude": "-14.643098",
            "longitude": "-80.968816"
        },
        "location_study": {
            "latitude": "-84.0534215",
            "longitude": "144.383503"
        },
        "location_work": {
            "latitude": "-13.8300325",
            "longitude": "101.956003"
        },
        "Age": "1",
        "travel_mode_study": "4"
    },
    "uuid": "d5f7eb50-6dec-48d1-86ff-ee4b96b583f7"
}
parameters = json.dumps(test_data)
response = requests.post(url, data=parameters)
```

> The above command returns JSON structured like this:

```json
{
    "status": "success",
    "type": "MobileUpdateData",
    "results": {
        "prompts": "No new prompt answers supplied.",
        "survey": "Survey answer for 0eb506b1-0411-4c91-8422-58ad44180272 upserted.",
        "coordinates": "No new coordinates data supplied."
    }
}
```

This endpoint is shared with updates for coordinates and modeprompts. This follows the design of the original API, however, it now requires only the necessary data to be submitted (i.e., the survey answers do not need to be sent in each request).

### HTTP Request

`POST http://<api.root.url>/mobile/v1/update`

### Query Parameters

| Parameter | Description                              |
| --------- | ---------------------------------------- |
| survey    | Dictionary containing the column names from the supplied schema and the user input values |
| uuid      | The user's cached installation uuid      |

## Updating Coordinates

```python
import requests

test_data = {
    "prompts": [],
    "uuid": "04399688-f7aa-4e7d-aeda-a54de210d843",
    "coordinates": [
        {
            "v_accuracy": 8,
            "mode_detected": 1,
            "acceleration_y": 0.9005850917305266,
            "h_accuracy": 3,
            "acceleration_x": 0.3757285807726195,
            "latitude": "45.5069920593",
            "timestamp": "2016-10-29T09:25:33-04:00",
            "speed": 48.07675588398361,
            "longitude": "-73.6316462699",
            "acceleration_z": 0.6462882776492511
        },
        {
            "v_accuracy": 19,
            "mode_detected": 3,
            "acceleration_y": 0.7147102339936264,
            "h_accuracy": 14,
            "acceleration_x": 0.7981422356509369,
            "latitude": "45.4709612219",
            "timestamp": "2016-10-29T09:25:48-04:00",
            "speed": 51.04449870001568,
            "longitude": "-73.6011947415",
            "acceleration_z": 0.47743632708330497
        },
        {
            "v_accuracy": 21,
            "mode_detected": 2,
            "acceleration_y": 0.6164638302522808,
            "h_accuracy": 1,
            "acceleration_x": 0.23135915741337032,
            "latitude": "45.5026375686",
            "timestamp": "2016-10-29T09:26:03-04:00",
            "speed": 53.47408048034383,
            "longitude": "-73.6364625799",
            "acceleration_z": 0.11871749871078396
        },
        {
            "v_accuracy": 44,
            "mode_detected": 1,
            "acceleration_y": 0.8849309929557423,
            "h_accuracy": 39,
            "acceleration_x": 0.2051143438611197,
            "latitude": "45.5138905076",
            "timestamp": "2016-10-29T09:26:18-04:00",
            "speed": 33.00381524750525,
            "longitude": "-73.6326201573",
            "acceleration_z": 0.6564783774960729
        },
        {
            "v_accuracy": 0,
            "mode_detected": 2,
            "acceleration_y": 0.8650806338748424,
            "h_accuracy": 25,
            "acceleration_x": 0.6513500120220839,
            "latitude": "45.4641170882",
            "timestamp": "2016-10-29T09:26:33-04:00",
            "speed": 40.212286896732074,
            "longitude": "-73.6115764412",
            "acceleration_z": 0.5778861925789305
        }
    ]
}

parameters = json.dumps(test_data)
response = requests.post(update_url, data=parameters)
```


> The above command returns JSON structured like this:

```json
{
    "status": "success",
    "type": "MobileUpdateData",
    "results": {
        "prompts": "No new prompt answers supplied.",
        "survey": "No new survey data supplied.",
        "coordinates": "New coordinates for 96fc430b-284d-45c5-9eef-bd2796a73d31 inserted."
    }
}

```

Allows application to record collected GPS points to server. This endpoint is shared with updates for survey responses and may be combined to a single request.

### HTTP Request

`POST http://<api.root.url>/mobile/v1/update`


| Parameter   | Description                              |
| ----------- | ---------------------------------------- |
| coordinates | Array of dictionaries containing GPS update information |
| prompts     | (Optional) array of mode prompt responses to record |
| user        | Dictionary containing `uuid` key and value (TODO: change this) |


## Updating Prompts

```python
import requests

test_data = {
    "prompts": [
        {
            "prompt_uuid": "0dd86866-9e00-474f-a24b-103431254726",
            "displayed_at": "2017-04-27T08:37:03-04:00",
            "recorded_at": "2017-04-27T08:37:03-04:00",
            "longitude": "-73.5769640073",
            "latitude": "45.4868670481",
            "answer": "Choice 1",
            "prompt_num": 0
        },
        {
            "prompt_uuid": "0dd86866-9e00-474f-a24b-103431254726",
            "displayed_at": "2017-04-28T07:24:25-04:00",
            "recorded_at": "2017-04-28T07:24:25-04:00",
            "longitude": "-73.6179342516",
            "latitude": "45.5205378578",
            "answer": ["Choice A", "Choice C"],
            "prompt_num": 1
        }
    ],
    "uuid": "bb9212b8-a608-48a2-962e-3ecb9632a12b",
    "coordinates": []
}

parameters = json.dumps(test_data)
response = requests.post(update_url, data=parameters)
```


> The above command returns JSON structured like this:

```json
{
    "status": "success",
    "type": "MobileUpdateData",
    "results": {
        "prompts": "New prompt answers for bb9212b8-a608-48a2-962e-3ecb9632a12b inserted.",
        "survey": "No new survey data supplied.",
        "coordinates": "No new coordinates data supplied."
    }
}
```

This endpoint is shared with updates for survey responses and may be combined to a single request. Each prompt requires the following information:
| Parameter    | Description                              |
| ------------ | ---------------------------------------- |
| prompt_uuid  | UUID for each prompt event, prompts displayed at during the same notification should share the same UUID |
| prompt_num   | Index (start=0) of the prompt being answered within an event's set |
| displayed_at | IS08601 representation of datetime when app created prompt event |
| recorded_at  | ISO8601 representation of datetime when user answered prompt event |
| longitude    | Longitude of where prompt event was created |
| latitude     | Latitude of where prompt event was created |
| answer       | List of plain-text strings for the user's prompt response |

Updated prompts may be resubmitted with the same format as long as the UUID remains consistent with the originally submitted prompt.

`POST http://<api.root.url>/mobile/v1/update`


| Parameter   | Description                              |
| ----------- | ---------------------------------------- |
| coordinates | (Optional) array of dictionaries containing GPS update informationgitlab |
| prompts     | Array of mode prompt responses to record |
| uuid        | `uuid` for mobile user                   |

## Updating Cancelled Prompts

```python
import requests

test_data = {
    "cancelledPrompts": [
        {
            "uuid": "c1d15413-b33d-4aaa-bf84-762517a3284b",
            "displayed_at": "2017-04-27T08:37:03-04:00",
            "cancelled_at": "2017-04-27T08:37:03-04:00",
            "longitude": "-73.5769640073",
            "latitude": "45.4868670481",
        },
        {
            "uuid": "e2dd9ab2-f859-4d44-b569-6791a4a2ed7b",
            "displayed_at": "2017-04-28T07:24:25-04:00",
            "cancelled_at": "2017-04-28T07:24:25-04:00",
            "longitude": "-73.6179342516",
            "latitude": "45.5205378578",
        }
    ],
    "uuid": "bb9212b8-a608-48a2-962e-3ecb9632a12b",
}

parameters = json.dumps(test_data)
response = requests.post(cancelled_prompts_url, data=parameters)
```


> The above command returns JSON structured like this:

```json
{
    "results": {
        "cancelledPrompts": "2 false prompts recorded."
    },
    "status": "success",
    "type": "MobileUpdateData"
}
```

This endpoint records times when a user is prompted about a trip end, but chooses to continue their trip and dismisses the prompt. Cancelled prompts are mutually exclusive events from answered prompts and should never share UUIDs. Previously submitted cancelled prompts can be upgraded by submitting an answered prompt set with the same UUID.


### HTTP Request

`POST http://api.testing.itinerum.ca/mobile/v1/update`


| Parameter        | Description                              |
| ---------------- | ---------------------------------------- |
| cancelledPrompts | Array of dictionaries containing timestamp and location of cancelled prompt |
| uuid             | `uuid` for mobile user                   |
