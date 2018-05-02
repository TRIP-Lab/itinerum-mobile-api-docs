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


# Create a New User

```python
import json
import requests

url = API_URL + '/create'

test_data = {
    "lang": "en",
    "surveyName": "test",
    "user": {
        "itinerumVersion": "12c",
        "uuid": "7077a34e-afe5-4c22-bd96-119256b7dc51",
        "osVersion": "80.23",
        "model": "iPhone 4s",
        "os": "ios"
    }
}
parameters = json.dumps(test_data)
response = requests.post(url, data=parameters)
```

A new user is created within a survey by supplying a generated UUID to identify the user within the database. This needs to be cached within the app as it is required for subsequent requests to the API. *Relying on the mobile application to generate the UUID is a carry-over from a legacy version of the platform, but retained for backwards compatibility. The possibility of UUID collision is exceedingly remote and was not deemed sufficient to change to a server-side implementation.*

The `avatar` path provides the relative URI of the survey's branding avatar. If no avatar is set, a `null` value will be returned and `defaultAvatar` should be referenced for the default Itinerum badge. In the example above, the full path would then be: `http://<api.root.url>/assets/static/avatars/93jf3.jpg`.

<aside class="notice">
The original implementation of the API implemented and documented key names inconsistently between camelCase and underscored key names. All network API calls should exclusively use camelCase naming and will be mandatory in future versions of the API. The <code>/mobile/v1</code> version is backwards compatible with both versions, but is deprecated will be disabled once it has been determined all active mobile apps are responding to the upcoming <code>/mobile/v2</code> API.</aside>

### HTTP Request

`POST http://<api.root.url>/mobile/v1/create`

### Query Parameters

| Parameter  | Description                                                  |
| ---------- | ------------------------------------------------------------ |
| user       | Dictionary containing `uuid`, `model`, `itinerumVersion`, `os`, `osVersion` |
| surveyName | Name of survey to associate the user                         |

## Parsing the Survey

> The above command returns JSON structured like this:

```json
{
    "uuid": "43f8cdaf-3e43-429d-beb2-94e8220a8a06",
    "lang": "en",
    "termsOfService": "terms of service text",
    "aboutText": "information about survey instance text",
    "contactEmail": "contact@email.com",
    "avatar": null,
    "defaultAvatar": "/assets/static/defaultAvatar.png",
    "surveyName": "tester",
    "surveyId": 22,
    "survey": [
        {
            "colName": "member_type",
            "prompt": "What is your primary occupation?",
            "id": 104,
            "fields": {
                "choices": ["0", "1", "2", "3", "4", "5"]
            }
        },
        {
            "colName": "travel_mode_work",
            "prompt": "How do you typically commute to your work location?",
            "id": 110,
            "fields": {
                "choices": ["0", "1", "2", "3", "4", "5", "6"]
            }
        },
        {
            "colName": "travel_mode_alt_work",
            "prompt": "Do you use any alternative mode of travel to work?",
            "id": 111,
            "fields": {
                "choices": ["0", "1", "2", "3", "4", "5", "6", "7"]
            }
        },
        {
            "colName": "travel_mode_study",
            "prompt": "How do you typically commute to your studies?",
            "id": 108,
            "fields": {
                "choices": ["0", "1", "2", "3", "4", "5", "6"]
            }
        },
        {
            "colName": "travel_mode_alt_study",
            "prompt": "Do you use any alternative mode of travel to your studies?",
            "id": 109,
            "fields": {
                "choices": ["0", "1", "2", "3", "4", "5", "6", "7"]
            }
        },
        {
            "colName": "Gender",
            "prompt": "What is your gender?",
            "id": 100,
            "fields": {
                "choices": ["0", "1", "2"]
            }
        },
        {
            "colName": "Age",
            "prompt": "What is your age bracket?",
            "id": 101,
            "fields": {
                "choices": ["0", "1", "2", "3", "4", "5"]
            }
        },
        {
            "colName": "dropdown",
            "prompt": "dropdown selection title",
            "id": 1,
            "fields": {
                "choices": ["Choice 1", "Choice 2"]
            }
        },
        {
            "colName": "checkbox",
            "prompt": "checkbox selection title",
            "id": 2,
            "fields": {
                "choices": ["Choice 1", "Choice 2"]
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
            "fields": {"latitude": null, "longitude": null}
        },
        {
            "colName": "address",
            "prompt": "address input title",
            "id": 4,
            "fields": {"latitude": null, "longitude": null}
        },
        {
            "colName": "location_study",
            "prompt": "Please enter your study location on the map",
            "id": 106,
            "fields": {"latitude": null, "longitude": null}
        },
        {
            "colName": "location_home",
            "prompt": "Please enter your home location on the map",
            "id": 105,
            "fields": {"latitude": null, "longitude": null}
        }
    ],
    "prompt": {
        "maxDays": 14,
        "prompts": [
            {
                "colName": "dropdown_prompt",
                "prompt": "dropdown prompt title",
                "id": 1,
                "choices": ["Choice 1", "Choice 2"]
            },
            {
                "colName": "checkbox_prompt",
                "prompt": "checkbox prompt title",
                "id": 2,
                "choices": ["Choice 1", "Choice 2"]
            }
        ],
        "maxPrompts": 20,
        "numPrompts": 0
    },
    "user": "New user successfully registered."
}
```

### Survey Questions

The `survey` values provides the ordered list of survey questions created using the dashboard's survey builder. The attributes for each survey question are detailed below.

| Key       | Value Types  | Description                                                  |
| --------- | ------------ | ------------------------------------------------------------ |
| `colName` | *string*     | The column label provided by admin users in the survey builder for output .csv files; currently also used by mobile apps as the question title -- this should like be reconsidered since formatting can be inconsistent |
| `prompt`  | *string*     | The question string for each survey question                 |
| `id`      | *integer*    | The generic field type ID as described in the generic or hardcoded fields tables |
| `fields`  | *dictionary* | A dictionary object containing the `field names` as keys from the generic  or hardcoded fields tables |

### Generic Fields

| id   | Field Names             | Description  |
| ---- | ----------------------- | ------------ |
| 1    | *choices*               | select one   |
| 2    | *choices*               | select many  |
| 3    | *number*                | number input |
| 4    | *latitude*, *longitude* | map/address  |
| 5    | *text*                  | text box     |

### Hardcoded Fields

Fields with an `id` greater or equal to 100 are hardcoded fields and mandatory within every survey. In the future, these will be pre-built, but optional components within the survey builder.

| id   | Hardcoded colName       | **Field Names (placeholder)** | Description                             |
| ---- | ----------------------- | ----------------------------- | --------------------------------------- |
| 100  | `Gender`                | *choices*                     | gender                                  |
| 101  | `Age`                   | *choices*                     | age                                     |
| 102  | `mode`                  | *choices*                     | primary mode (unused)                   |
| 103  | `Email`                 | *choices*                     | email                                   |
| 104  | `member_type`           | *choices*                     | primary occupation                      |
| 105  | `location_home`         | *latitude*, *longitude*       | mark user's home location on map        |
| 106  | `location_study`        | *latitude*, *longitude*       | mark user's study location on map       |
| 107  | `location_work`         | *latitude*, *longitude*       | mark user's work location on map        |
| 108  | `travel_mode_study`     | *choices*                     | primary travel mode to study location   |
| 109  | `travel_mode_alt_study` | *choices*                     | alternate travel mode to study location |
| 110  | `travel_mode_work`      | *choices*                     | primary travel mode work location       |
| 111  | `travel_mode_alt_work`  | *choices*                     | alternate travel mode to work location  |


# Updating the Database
## Answering the Survey

```python
import requests
import json

test_data = {
    "survey": {
        "checkbox": ["Choice 1"],
        "dropdown": "Choice 2",
        "Gender": 1,
        "travel_mode_work": 5,
        "address": {
            "latitude": -12.5665545,
            "longitude": -41.789760
        },
        "number": 5789,
        "travel_mode_alt_work": 7,
        "Email": "vpollich@tillman-heathcote.com",
        "travel_mode_alt_study": 0,
        "member_type": 0,
        "text": "Cumque nemo occaecati aliquid nam iusto rem itaque. Aperiam saepe itaque quas odit accusantium. Recusandae maiores consequatur esse veniam sequi odit.",
        "location_home": {
            "latitude": -14.643098,
            "longitude": -80.968816
        },
        "location_study": {
            "latitude": -84.0534215,
            "longitude": 144.383503
        },
        "location_work": {
            "latitude": -13.8300325,
            "longitude": 101.956003
        },
        "Age": 1,
        "travel_mode_study": 4
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

<aside class="notice">Keys within <code>survey</code> attribute do not adhere to the camelCase requirements since these keys will be determined by the user via the dashboard's survey builder. The original hardcoded questions also retain underscores to introduce less breaking changes and eventually these will be deprecated in favor of new <b><a href="#parsing-the-survey">generic fields</a></b> with the same purpose.

<br></br>

<i>This seemed an acceptable trade-off since they are dynamic fields and will not be relied on by the app except in special hardcoded circumstances for existing tailored mobile apps.</i></aside>

This endpoint is shared with updates for coordinates, prompts, and cancelled prompts. This follows the design of the original API, however, it now requires only the necessary data to be submitted (i.e., the survey answers do not need to be sent in each request).

### HTTP Request

`POST http://<api.root.url>/mobile/v1/update`

### Query Parameters

| Parameter | Description                                                  |
| --------- | ------------------------------------------------------------ |
| `uuid`    | The user's cached installation uuid                          |
| `survey`  | Dictionary containing the column names from the supplied schema and the user input values |

## Adding Coordinates

```python
import requests

test_data = {
    "uuid": "04399688-f7aa-4e7d-aeda-a54de210d843",
    "coordinates": [
        {
            "latitude": 45.5069920593,
            "longitude": -73.6316462699,
            "altitude": 32.34234,
            "speed": 48.07675588398361,
            "direction": 164.3948,
            "hAccuracy": 3,
            "vAccuracy": 8,
            "accelerationX": 0.3757285807726195,
            "accelerationY": 0.9005850917305266,
            "accelerationZ": 0.6462882776492511,
            "modeDetected": 1,
            "pointType": 3,
            "timestamp": "2016-10-29T09:25:33-04:00",
        },
        {
            "latitude": 45.4709612219,
            "longitude": -73.6011947415,
            "altitude": 31.82743,
            "speed": 51.04449870001568,
            "direction": 162.83454384,
			"hAccuracy": 14,
            "vAccuracy": 19,
            "modeDetected": 3,
            "accelerationX": 0.7981422356509369,
            "accelerationY": 0.7147102339936264,
            "accelerationZ": 0.47743632708330497,
            "modeDetected": 1,
            "pointType": 1,
            "timestamp": "2016-10-29T09:25:48-04:00",
        },
        {
            "latitude": 45.5026375686,
            "longitude": -73.6364625799,
            "altitude": 33.11732,
            "speed": 53.47408048034383,
            "direction": 163.28343,
            "hAccuracy": 1,
            "vAccuracy": 21,
            "accelerationX": 0.23135915741337032,
            "accelerationY": 0.6164638302522808,
            "accelerationZ": 0.11871749871078396,
            "modeDetected": 2,
            "pointType": 5,
            "timestamp": "2016-10-29T09:26:03-04:00",
        },
        ...
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

Allows application to send recorded GPS points to the server. This endpoint is shared with updates for survey responses and may be combined into a single request.

### HTTP Request

`POST http://<api.root.url>/mobile/v1/update`


| Parameter          | Description                                             |
| ------------------ | ------------------------------------------------------- |
| `uuid`             | The user's cached installation uuid                     |
| `coordinates`      | Array of dictionaries containing GPS points information |
| `cancelledPrompts` | (Optional) array of cancelled prompts dictionaries      |
| `prompts`          | (Optional) array of prompt responses dictionaries       |


## Upserting Prompts

```python
import requests

test_data = {
    "prompts": [
        {
            "uuid": "0dd86866-9e00-474f-a24b-103431254726",
            "displayedAt": "2017-04-27T08:37:03-04:00",
            "recordedAt": "2017-04-27T08:37:03-04:00",
            "longitude": -73.5769640073,
            "latitude": 45.4868670481,
            "answer": "Choice 1",
            "prompt_num": 0
        },
        {
            "uuid": "0dd86866-9e00-474f-a24b-103431254726",
            "displayedAt": "2017-04-28T07:24:25-04:00",
            "recordedAt": "2017-04-28T07:24:25-04:00",
            "longitude": -73.6179342516,
            "latitude": 45.5205378578,
            "answer": ["Choice A", "Choice C"],
            "promptNum": 1
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

Prompt question types use the same [generic fields](#parsing-the-survey) format as the survey questions. Prompt question types can be either dropdown/"select one" (1), checkboxes/"select many" (2), or text box/"unformatted text field" (5). This endpoint is shared with updates for survey responses and may be combined to a single request. Each prompt requires the following information:

| Parameter     | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| `uuid`        | UUID for each prompt event, prompts displayed during the same prompt event (i.e., app has detected that user has made a stop) should share the same UUID |
| `promptNum`   | Index (start=0) of the prompt being answered during a prompt event |
| `displayedAt` | IS08601 representation of datetime when app created prompt event |
| `recordedAt`  | ISO8601 representation of datetime when user answered prompt event |
| `longitude`   | Longitude of where prompt event was created                  |
| `latitude`    | Latitude of where prompt event was created                   |
| `answer`      | List of plain-text strings for the user's prompt response    |

Updated prompts may be resubmitted with the same format as long as the UUID remains consistent with the originally submitted prompt.

`POST http://<api.root.url>/mobile/v1/update`


| Parameter          | Description                                                  |
| ------------------ | ------------------------------------------------------------ |
| `uuid`             | `uuid` for mobile user (different from prompt `uuid` above)  |
| `prompts`          | Array of user's prompt responses                             |
| `cancelledPrompts` | (Optional) array of cancelled prompts dictionaries           |
| `coordinates`      | (Optional) array of dictionaries containing GPS points information |

## Adding Cancelled Prompts

```python
import requests

test_data = {
    "cancelledPrompts": [
        {
            "uuid": "c1d15413-b33d-4aaa-bf84-762517a3284b",
            "displayedAt": "2017-04-27T08:37:03-04:00",
            "cancelledAt": "2017-04-27T08:37:03-04:00",
            "longitude": -73.5769640073,
            "latitude": 45.4868670481,
        },
        {
            "uuid": "e2dd9ab2-f859-4d44-b569-6791a4a2ed7b",
            "displayedAt": "2017-04-28T07:24:25-04:00",
            "cancelledAt": "2017-04-28T07:24:25-04:00",
            "longitude": -73.6179342516,
            "latitude": 45.5205378578,
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

This endpoint records times when a user is prompted about a trip end, but chooses to continue their trip and dismisses the prompt. Cancelled prompts are mutually exclusive events from answered prompts and should never share UUIDs. Previously submitted cancelled prompts will be upgraded automatically by submitting an answered prompt set with the same UUID.


### HTTP Request

`POST http://<api.root.url>/mobile/v1/update`


| Parameter          | Description                                                  |
| ------------------ | ------------------------------------------------------------ |
| `uuid`             | `uuid` for mobile user                                       |
| `cancelledPrompts` | Array of cancelled prompts dictionaries                      |
| `coordinates`      | (Optional) array of dictionaries containing GPS points information |
| `prompts`          | (Optional) array of prompt responses dictionaries            |
