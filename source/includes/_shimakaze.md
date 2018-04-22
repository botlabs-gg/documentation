# Reputation API (shimakaze)

This API can be used to assign reputation to a user, while automatically checking for limits such as max reputation a user may give/receive per day as well as the maximum reputation a user may get totally. 

You can also customize those limits to fit your needs. The cooldown of per reputation is also customizable!

Codename: `shimakaze`

Timestamps are always in `UTC`

## User object

>Json representation of user object

```json
{
   "reputation":477,
   "cooldown":[
      "2018-03-31T12:05:19.851Z",
      "2018-03-31T12:05:35.008Z"
   ],
   "givenReputation":[
      "2018-04-16T14:16:26.186Z",
      "2018-04-16T14:16:28.062Z"
   ],
   "userId":"186221791792857088",
   "botId":"388799526103941121",
   "accountId":"H1e7U2i8f"
}
```

| name                     | type       | description                                                                                                  | optional |
|--------------------------|------------|--------------------------------------------------------------------------------------------------------------|----------|
| reputation               | integer    | current reputation of user                                                                                   |          |
| cooldown                 | date[]     | Array of timestamps referring to the last time(s) this user has given reputation to another user             |          |
| givenReputation          | date[]     | Array of timestamps referring to the last time(s) this user has received reputation from another user        |          |
| userId                   | string     | Id of the user that was passed in the first call to take or give reputation to the user                      |          |
| botId                    | string     | Id of the bot that was passed in the first call to take or give reputation to the user                       |          |
| accountId                | string     | Internal id associated with the token calling the API                                                        |          |
| availableReputations     | integer    | How many reputations the user may give out                                                                   | x        |
| nextAvailableReputations | integer[] | Array of timestamps referring to the remaining cooldown time until the user can give out reputation from now | x        |

## Reputation settings object

>Json representation of settings object

```json
{
   "reputationPerDay":2,
   "maximumReputation":0,
   "maximumReputationReceivedDay":0,
   "reputationCooldown":"86400"
}
```

| name                         | type    | description                                                      | default       |
|------------------------------|---------|------------------------------------------------------------------|---------------|
| reputationPerDay             | integer | Number of reputations a user may give out per reputationCooldown | 2             |
| maximumReputation            | integer | The maximum reputation a user may receive                        | 0 (disabled)  |
| maximumReputationReceivedDay | integer | The maximum reputation a user may receive per day                | 0 (disabled)  |
| reputationCooldown           | integer | Cooldown per reputation, this is set to time in seconds          | 86400 (1 day) |


## Get reputation of user

> Response

```json
{
    "user": {
        "userId": "128392910574977024",
        "botId": "388799526103941121",
        "accountId": "H1e7U2i8f",
        "givenReputation": [
            "2018-04-17T15:33:44.778Z",
            "2018-04-17T15:33:48.118Z"
        ],
        "cooldown": [
            "2018-04-17T15:33:32.842Z",
            "2018-04-17T15:33:35.415Z"
        ],
        "reputation": 471,
        "availableReputations": 0,
        "nextAvailableReputations": [
            85671127,
            85673700
        ]
    },
    "date": "2018-04-17T15:45:41.675Z",
    "status": 200
}
```

### Endpoint `/reputation/:bot_id/:user_id`
### Methods `GET`

This endpoint allows you to see the current reputation a user has and when they are able to give out reputation again.


To be able to compute the time from now until the user can give out reputation again, this endpoint provides you with the fields `nextAvailableReputations` as well as `date`.

### Response fields

| name   | type    | description                      |
|--------|---------|----------------------------------|
| user   | User    | User object with optional fields |
| date   | date    | Current server time in UTC       |
| status | integer | HTTP status code                 |

## Give reputation to user

>Request body

```json 

{"source_user":"y"}

```

>Response

```json
{
    "status": 200,
    "date": "2018-04-17T17:03:58.865Z",
    "code": 0,
    "message": "Successfully gave one reputation to the targetUser",
    "sourceUser": {
        "reputation": 6,
        "cooldown": [
            "2018-04-17T17:03:58.864Z"
        ],
        "givenReputation": [
            "2018-04-17T16:17:10.370Z",
            "2018-04-17T16:17:11.758Z"
        ],
        "userId": "y",
        "botId": "x",
        "accountId": "S1WJTqQhf"
    },
    "targetUser": {
        "reputation": 6,
        "cooldown": [
            "2018-04-17T16:29:32.432Z",
            "2018-04-17T16:29:43.303Z",
            "2018-04-17T16:29:44.658Z"
        ],
        "givenReputation": [
            "2018-04-17T16:13:34.916Z",
            "2018-04-17T16:15:17.048Z"
        ],
        "userId": "x",
        "botId": "x",
        "accountId": "S1WJTqQhf"
    }
}
```

>Error response

```json
{
    "status": 403,
    "date": "2018-04-17T17:18:26.727Z",
    "code": 1,
    "message": "The user used all of his reputations.",
    "user": {
        "reputation": 6,
        "cooldown": [
            "2018-04-17T17:18:24.614Z",
            "2018-04-17T17:18:25.428Z"
        ],
        "givenReputation": [
            "2018-04-17T16:17:10.370Z",
            "2018-04-17T16:17:11.758Z"
        ],
        "userId": "y",
        "botId": "x",
        "accountId": "S1WJTqQhf"
    }
}
```

### Endpoint `/reputation/:bot_id/:target_user_id`
### Methods `POST`

This endpoint makes one user (the source user) give one reputation point to the target user.

It also checks `cooldown`, as well as `maximumReputation`, `maximumReputationReceivedDay` and `reputationPerDay`.

### Request fields

| name        | type   | description       |
|-------------|--------|-------------------|
| source_user | string | Id of source_user |

### Response fields

| name       | type    | description                                |
|------------|---------|--------------------------------------------|
| status     | integer | HTTP status code                           |
| date       | date    | Current server time in UTC                 |
| code       | integer | Return code, see below for possible errors |
| message    | string  | Informational message                      |
| sourceUser | User    | User that gave a reputation point          |
| targetUser | User    | User that received a reputation point      |

### Error codes

| code | HTTP code | description                                                  | user        |
|------|-----------|--------------------------------------------------------------|-------------|
| 0    | 200       | Successful                                                   | -           |
| 1    | 403       | The user used all of his reputations and hit the cooldown    | source_user |
| 2    | 403       | The user received the maximum amount of reputation for today | target_user |
| 3    | 403       | The user reached the maximum possible amount of reputation   | target_user |

## Reset user reputation

>Response

```json
{
    "user": {
        "reputation": 0,
        "cooldown": [],
        "givenReputation": [
            "2018-04-17T16:13:34.916Z",
            "2018-04-17T16:15:17.048Z"
        ],
        "userId": "x",
        "botId": "x",
        "accountId": "S1WJTqQhf"
    },
    "status": 200
}
```

Did a user cheat reputation ? Just reset it!

### Endpoint `/reputation/:bot_id/:target_user_id/reset`
### Methods `POST`

### Query parameters

| name     | type    | description                                         | default |
|----------|---------|-----------------------------------------------------|---------|
| cooldown | boolean | Whether to reset the cooldown field of the user too | false   |


### Response fields

| name   | type    | description                      |
|--------|---------|----------------------------------|
| user   | User    | User object                      |
| status | integer | HTTP status code                 |

## Increase user reputation

>Request body

```json 

{"increase":1}

```

>Response

```json
{
    "user": {
        "reputation": 2,
        "cooldown": [],
        "givenReputation": [
            "2018-04-17T16:13:34.916Z",
            "2018-04-17T16:15:17.048Z",
            "2018-04-17T16:30:27.825Z",
            "2018-04-17T16:30:30.833Z",
            "2018-04-17T16:30:31.401Z",
            "2018-04-17T17:03:58.861Z",
            "2018-04-17T17:18:24.613Z",
            "2018-04-17T17:18:25.427Z",
            "2018-04-17T17:18:26.111Z"
        ],
        "userId": "x",
        "botId": "x",
        "accountId": "S1WJTqQhf"
    },
    "status": 200
}
```

### Endpoint `/reputation/:bot_id/:target_user_id/increase`
### Methods `POST`

Increases the reputation of the user, useful for automated systems, rewarding a user for voting or for assigning a bought reputation point.

### Request fields

| name     | type    | description                                                |
|----------|---------|------------------------------------------------------------|
| increase | integer | Increases the reputation of the user by the passed integer |

### Response fields

| name   | type    | description                      |
|--------|---------|----------------------------------|
| user   | User    | User object                      |
| status | integer | HTTP status code                 |


## Decrease user reputation

>Request body

```json 

{"decrease":1}

```

>Response

```json
{
    "user": {
        "reputation": 1,
        "cooldown": [],
        "givenReputation": [
            "2018-04-17T16:13:34.916Z",
            "2018-04-17T16:15:17.048Z",
            "2018-04-17T16:30:27.825Z",
            "2018-04-17T16:30:30.833Z",
            "2018-04-17T16:30:31.401Z",
            "2018-04-17T17:03:58.861Z",
            "2018-04-17T17:18:24.613Z",
            "2018-04-17T17:18:25.427Z",
            "2018-04-17T17:18:26.111Z"
        ],
        "userId": "x",
        "botId": "x",
        "accountId": "S1WJTqQhf"
    },
    "status": 200
}
```

### Endpoint `/reputation/:bot_id/:target_user_id/decrease`
### Methods `POST`

Decreases the reputation of the user, useful for automated systems, allowing users to "steal" reputation or similar use-cases.

### Request fields

| name     | type    | description                                                |
|----------|---------|------------------------------------------------------------|
| decrease | integer | Decreases the reputation of the user by the passed integer |

### Response fields

| name   | type    | description                      |
|--------|---------|----------------------------------|
| user   | User    | User object                      |
| status | integer | HTTP status code                 |

## Get settings

>Response

```json
{
    "settings": {
        "reputationPerDay": 2,
        "maximumReputation": 0,
        "maximumReputationReceivedDay": 0,
        "reputationCooldown": 86400,
        "accountId": "S1WJTqQhf"
    },
    "status": 200
}
```


### Endpoint `/reputation/settings`
### Methods `GET`

Allows you to get the currently active settings for your token

### Response fields

| name     | type     | description      |
|----------|----------|------------------|
| settings | Settings | Settings object  |
| status   | integer  | HTTP status code |


## Set settings

>Request body

```json
{
   "reputationPerDay":5,
   "maximumReputation":0,
   "maximumReputationReceivedDay":0,
   "reputationCooldown":86400
}
```

>Response

```json
{
    "settings": {
        "reputationPerDay": 2,
        "maximumReputation": 0,
        "maximumReputationReceivedDay": 0,
        "reputationCooldown": 86400,
        "accountId": "S1WJTqQhf"
    },
    "status": 200
}
```

### Endpoint `/reputation/settings`
### Methods `POST`

Allows you to update your settings, none of the fields are required.
### Request fields


| name                         | type    | description                                                              | default       |
|------------------------------|---------|--------------------------------------------------------------------------|---------------|
| reputationPerDay             | integer | Number of reputations a user may give out per reputationCooldown         | 2             |
| maximumReputation            | integer | The maximum reputation a user may receive                                | 0 (disabled)  |
| maximumReputationReceivedDay | integer | The maximum reputation a user may receive per day                        | 0 (disabled)  |
| reputationCooldown           | integer | Cooldown per reputation, this is set to time in seconds (must be >= 300) | 86400 (1 day) |


### Response fields

| name     | type     | description      |
|----------|----------|------------------|
| settings | Settings | Settings object  |
| status   | integer  | HTTP status code |

