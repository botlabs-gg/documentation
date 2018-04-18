# Settings API (tama)

This API can be used to store abitrary settings of your bot like prefix, user settings and more.

The routes are generally based on the following schema:

Settings /`type`/`id`

Sub-Settings /`type`/`id`/`subtype`/`subid`

You are **able** to list sub-settings, but **unable** to list settings.

With this in mind, you could use routes similar to this:

`/guilds/300407204987666432` -> store guild wide settings

`/guilds/300407204987666432/channels/300407204987666432` -> store settings for a channel

`/guilds/300407204987666432/members/128392910574977024` -> store settings for a member

## Settings object

>Json representation of settings object

```json
{
   "id":"339114875769061409",
   "type":"guilds",
   "accountId":"H1e7U2i8f",
   "data":{
      "prefix":"poi",
      "smartrec":true
   }
}
```

| name      | type   | description                                           |
|-----------|--------|-------------------------------------------------------|
| id        | string | Id of this setting                                    |
| type      | string | Type of this setting                                  |
| accountId | string | Internal id associated with the token calling the API |
| data      | object | The user-provided data of this setting                |

## Sub-Settings object

>Json representation of sub-settings object

```json
{
   "id":"339114875769061409",
   "type":"guilds",
   "accountId":"HyxjFGfPb",
   "data":{
      "name":"general"
   },
   "subId":"300407204987666432",
   "subType":"channels"
}
```

| name      | type   | description                                           |
|-----------|--------|-------------------------------------------------------|
| id        | string | Id of the parent setting                              |
| type      | string | Type of the parent setting                            |
| accountId | string | Internal id associated with the token calling the API |
| data      | object | The user-provided data of this sub-setting            |
| subId     | string | Id of this sub-setting                                |
| subType   | string | Type of this sub-setting                              |

## Get setting

>Response

```json
{
    "status": 200,
    "setting": {
        "id": "339114875769061409",
        "type": "guilds",
        "accountId": "H1e7U2i8f",
        "data": {
            "prefix": "poi",
            "smartrec": true
        }
    }
}
```

This endpoint allows you to fetch a setting by type and id.
<aside class="notice">
<b>Please make sure to properly cache the setting after you fetched it,
   especially for something like prefixes,
   make sure to use a responsible caching behaviour of around 1-5 minutes,
   to make sure that you don't spam the API.</b>
</aside>


### Endpoint `/settings/:type/:id`
### Method `GET`

### Response fields

| name    | type    | description      |
|---------|---------|------------------|
| setting | Setting | Setting object   |
| status  | integer | HTTP status code |


## Create/Update setting

>Request body example

```json
{
   "prefix":"poi",
   "smartrec":true
}
```

>Response

```json
{
    "status": 200,
    "setting": {
        "id": "339114875769061409",
        "type": "guilds",
        "accountId": "HyxjFGfPb",
        "data": {
            "prefix": "poi",
            "smartrec": true
        }
    }
}
```
This endpoint allows you to create a new setting/update an existing one.

**It overwrites the existing data with the data passed by the user.**

So make sure to include changed and not-changed fields so you don't overwrite anything precious!

### Endpoint `/settings/:type/:id`
### Method `POST`

### Request fields

You may send any kind of json as long as the resulting http request body stays smaller than 10 kilobytes.

### Response fields

| name    | type    | description      |
|---------|---------|------------------|
| setting | Setting | Setting object   |
| status  | integer | HTTP status code |


## Delete setting

>Response

```json
{
    "status": 200,
    "message": "Setting deleted",
    "setting": {
        "id": "339114875769061409",
        "type": "guilds",
        "accountId": "HyxjFGfPb",
        "data": {
            "prefix": "awuu"
        }
    }
}
```
This endpoint allows you to delete a setting
### Endpoint `/settings/:type/:id`
### Method `DELETE`

### Response fields

| name    | type    | description           |
|---------|---------|-----------------------|
| setting | Setting | Setting object        |
| status  | integer | HTTP status code      |
| message | string  | Informational message |


## List sub-settings

>Response

```json
{
    "status": 200,
    "subsettings": [
        {
            "id": "339114875769061409",
            "type": "guilds",
            "accountId": "HyxjFGfPb",
            "data": {
                "name": "general"
            },
            "subId": "300407204987666432",
            "subType": "channels"
        }
    ]
}
```

This endpoint allows you to get a list of sub-settings for a sub-setting type.

<aside class="notice">
<b>You don't have to create a parent setting to be able to use the sub-settings of it</b>
</aside>

### Endpoint `/settings/:type/:id/:subtype`
### Method `GET`

### Response fields

| name        | type          | description          |
|-------------|---------------|----------------------|
| status      | integer       | HTTP status code     |
| subsettings | Sub-setting[] | Array of subsettings |

## Get sub-setting

>Response

```json
{
    "status": 200,
    "subsetting": {
        "id": "339114875769061409",
        "type": "guilds",
        "accountId": "HyxjFGfPb",
        "data": {
            "name": "general"
        },
        "subId": "300407204987666432",
        "subType": "channels"
    }
}
```

This endpoint allows you to get a single sub-setting by it's subId.

### Endpoint `/settings/:type/:id/:subtype/:subid`
### Method `GET`

### Response fields

| name        | type          | description          |
|-------------|---------------|----------------------|
| status      | integer       | HTTP status code     |
| subsetting  | Sub-setting   | Sub-setting object   |


## Create/Update sub-setting

>Request body

```json
{
	"name":"general"
}
```

>Response 

```json
{
    "status": 200,
    "subsetting": {
        "id": "339114875769061409",
        "type": "guilds",
        "accountId": "HyxjFGfPb",
        "data": {
            "name": "general"
        },
        "subId": "300407204987666432",
        "subType": "channels"
    }
}
```

This endpoint allows you to create a new sub-setting/update an existing one.

**It overwrites the existing data with the data passed by the user.**

So make sure to include changed and not-changed fields so you don't overwrite anything precious!


### Endpoint `/settings/:type/:id/:subtype/:subid`
### Method `POST`

### Request fields

You may send any kind of json as long as the resulting http request body stays smaller than 10 kilobytes.

### Response fields

| name       | type        | description        |
|------------|-------------|--------------------|
| subsetting | Sub-Setting | Sub-Setting object |
| status     | integer     | HTTP status code   |


## Delete sub-setting
>Response

```json
{
    "status": 200,
    "message": "Subsetting deleted",
    "subsetting": {
        "id": "339114875769061409",
        "type": "guilds",
        "accountId": "HyxjFGfPb",
        "data": {
            "name": "general"
        },
        "subId": "300407204987666432",
        "subType": "channels"
    }
}
```

This endpoint allows you to delete a sub-setting

### Endpoint `/settings/:type/:id/:subtype/:subid`
### Method `DELETE`

### Response fields

| name       | type        | description           |
|------------|-------------|-----------------------|
| subsetting | Sub-Setting | Sub-Setting object    |
| status     | integer     | HTTP status code      |
| message    | string      | Informational message |
