# Image API (toph)

This api provides images (reactions, characters, animals) for various categories.

Codename: `toph`

#### Default permissions:

|Permission name|description                            |environment |
|---------------|---------------------------------------|------------|
|image_data     | Allows you to access basic image data | staging    |
|image_data     | Allows you to access basic image data | production |


#### Image API CDN Endpoints:

The image API uses the weeb.sh CDN to provide it's reaction images,
depending on the environment of the image API you are using,
the base url of the returned image may be one of the following:

| endpoint                           | environment | description                           |
|------------------------------------|-------------|---------------------------------------|
| https://cdn.weeb.sh/images         | production  | The production CDN endpoint for toph  |
| https://cdn.weeb.sh/staging-images | staging     | The staging CDN endpoint for toph     |
| https://cdn.weeb.sh/dev-images     | development | The development CDN endpoint for toph |

## Image Object

>Json representation of image object

```json
{
    "id": "BJZfMrXwb",
    "type": "awoo",
    "baseType": "awoo",
    "nsfw": false,
    "fileType": "gif",
    "mimeType": "image/gif",
    "tags": [],
    "url": "https://cdn.weeb.sh/images/BJZfMrXwb.gif",
    "hidden": false,
    "account": "HyxjFGfPb"
}
```

| name     | type                 | description                                                                            |
|----------|----------------------|----------------------------------------------------------------------------------------|
| id       | string               | unique id of the image                                                                 |
| type     | string               | type/category of the image, this is what's used to show the list of types in /types    |
| baseType | string               | ^                                                                                      |
| nsfw     | boolean              | whether this image has content that could be considered NSFW (not safe for work)       |
| fileType | string               | file extension of the image                                                            |
| mimeType | string               | mime type of the image                                                                 |
| tags     | array of tag objects | tags associated with this image                                                        |
| url      | string               | full url used to load the image, you can safely hotlink the image to your site/service |
| hidden   | boolean              | whether this image can only be seen by the uploader                                  
| ?source | string | source url of the image |
| account  | string               | id of the account that uploaded that image  |

## Upload Image
> Response

```json
{
"status":200,
"file":{
	"id": "BJZfMrXwb",
    "type": "awoo",
    "baseType": "awoo",
    "nsfw": false,
    "fileType": "gif",
    "mimeType": "image/gif",
    "tags": [],
    "url": "https://cdn.weeb.sh/images/BJZfMrXwb.gif",
    "hidden": false,
    "account": "HyxjFGfPb"
	}
}
```


### Endpoint `/images/upload`
### Methods `POST`

### Required Permissions

| Permission name 	   | description 									|
|----------------------|------------------------------------------------|
| upload_image         | allows you to upload public and private images |
| upload_image_private | allows you to upload private images           |

### Content Types
| Name | description |
|----------------------|------------------------------------------------|
| application/json    | When uploading images via url   |
| multipart/form-data | When uploading images from disk |

### Payload

| name     | type        | description                                                                                                   |
|----------|-------------|----------------------|
| file     | File Buffer | Buffer containing the image data of the image you want to upload (takes priority over url argument)           |
| url      | string      | Url pointing directly at the image you want to upload, you may only use file or url                           |
| baseType | string      | type of the image, this can be viewed as the category of an image                                             |
| hidden   | boolean     | If the uploaded image should be hidden                                                                        |
| tags     | string      | comma seperated list of tags that should be added to the image, they inherit the hidden property of the image |
| nsfw     | boolean     | whether this image has content that could be considered NSFW (not safe for work)                              |
| source   | string      | Url pointing to the original source of the image                                                 
|

You will get back a json response with a regular image object wrapped with a file key


## Image types

>Example response

```json
{
    "status": 200,
    "types": [
        "awoo",
        "bang",
        "blush",
        "clagwimoth"
    ],
    "preview": [
	     {
            "url": "https://cdn.weeb.sh/images/BJZfMrXwb.gif",
            "id": "BJZfMrXwb",
            "fileType": "gif",
            "baseType": "awoo",
            "type": "awoo"
        },
        {
            "url": "https://cdn.weeb.sh/images/rJmPWI7wW.gif",
            "id": "rJmPWI7wW",
            "fileType": "gif",
            "baseType": "bang",
            "type": "bang"
        },
        {
            "url": "https://cdn.weeb.sh/images/HklJGIXPW.gif",
            "id": "HklJGIXPW",
            "fileType": "gif",
            "baseType": "blush",
            "type": "blush"
        },
        {
            "url": "https://cdn.weeb.sh/images/HyNYMIXDb.png",
            "id": "HyNYMIXDb",
            "fileType": "png",
            "baseType": "clagwimoth",
            "type": "clagwimoth"
        }
	]
}
```

### endpoint `/images/types`
### methods `GET`
### required permissions
|Permission name| description|
|------------|---------------------------------------|
| image_data | Allows you to access basic image data |

### query parameters
| name    | type    | description                                                                               | default                                                         |
|---------|---------|------------------------|--------------------------------|
| hidden  | boolean | if true, you only get back hidden images you uploaded                                                             | returns types from public images and hidden images which you uploaded |
| nsfw    | string  | When false, no types from nsfw images will be returned, true returns types from nsfw and non-nsfw images, only returns only types from nsfw images | false                                                           |
| preview | boolean | Get a preview image for each type                                                                                 | false                                                           |

You will get back a list of type strings wrapped in an object similar to this:
Preview will contain a list of partial image objects which you can find below.
The preview image for a type does not change as long as the image itself isn't deleted.

## Image tags

>Example response

```json
{
    "status": 200,
    "tags": [
        "nuzzle",
        "cuddle",
        "momiji inubashiri",
        "wan",
        "astolfo",
        "facedesk",
        "everyone"
    ]
}
```

### endpoint  `/images/tags`
### method  `GET`
### required permissions
|Permission name| description|
|------------|---------------------------------------|
| image_data | Allows you to access basic image data |


### query parameters

| name   | type    | description                                                                                                                                            | default                                        |
|--------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------|
| hidden | boolean | if true, you only get back hidden tags you added                                                                                                       | returns public tags and private ones you added |
| nsfw   | string  | When false, no tags coming from nsfw images will be returned, true returns tags from nsfw and non-nsfw images, only returns only tags from nsfw images | false                                          |

returns a list of tags in string format wrapped with a tags key

## Random image

>Example response

```json
{
    "id": "BJZfMrXwb",
    "type": "awoo",
    "baseType": "awoo",
    "nsfw": false,
    "fileType": "gif",
    "mimeType": "image/gif",
    "tags": [],
    "url": "https://cdn.weeb.sh/images/BJZfMrXwb.gif",
    "hidden": false,
    "account": "HyxjFGfPb"
}
```

### endpoint `/images/random`
### method `GET`
### required permissions
|Permission name| description|
|------------|---------------------------------------|
| image_data | Allows you to access basic image data |

### query parameters

| name     | type    | description                                                                                                                                        | default                                      |
|----------|---------|----------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------|
| type     | string  | type of the image you want to get Either Type or Tags is mandatory, but you can combine them                                                       | -                                            |
| tags     | string  | comma seperated list of the tags the image should have                                                                                             | -                                            |
| nsfw     | string  | When false, no types from nsfw images will be returned, true returns types from nsfw and non-nsfw images, only returns only types from nsfw images | false                                        |
| hidden   | boolean | When false you only get public images, true will only give you hidden images uploaded by yourself                                                  | public images and hidden images you uploaded |
| filetype | string  | Filetype of the image, may either be jpg/jpeg, png or gif. jpeg and jpg are treated like being the same.                                           | -                                            |

returns a random image

## Image info

>Example response

```json
{
    "id": "BJZfMrXwb",
    "type": "awoo",
    "baseType": "awoo",
    "nsfw": false,
    "fileType": "gif",
    "mimeType": "image/gif",
    "tags": [],
    "url": "https://cdn.weeb.sh/images/BJZfMrXwb.gif",
    "hidden": false,
    "account": "HyxjFGfPb"
}
```

### endpoint `/info/:image_id`
### method `GET`

### required permissions
|Permission name| description|
|------------|---------------------------------------|
| image_data | Allows you to access basic image data |

Gives you the image object for an id

## Add tags to image
### endpoint `/info/:image_id/tags`
### method `POST`
|Permission name| description|
|------------|---------------------------------------|
| image_tags     | Add tags to an image|



## Remove tags from image
### endpoint `/info/:image_id/tags`
### method `DELETE`
### required permissions
|Permission name| description|
|------------|---------------------------------------|
| image_tags_delete     | Delete tags from an image|


## Delete image
### endpoint `/info/:image_id`
### method `DELETE`

### required permissions
|Permission name| description|
|------------|---------------------------------------|
| image_delete     | Delete images from any user  |
| image_delete_private | Delete hidden images you uploaded |

<aside class="notice">
<b>The endpoints down below will likely change in the future.</b>
</aside>
## List images
#### endpoint `/list`
#### method `GET`


### required permissions
|Permission name| description|
|------------|---------------------------------------|
| image_list_all | List images from all users |


## List images user
### endpoint `/list/:account_id`
### method `GET`

### required permissions
|Permission name| description|
|------------|---------------------------------------|
| image_list     | List images you uploaded   |
| image_list_all | List images from all users |
