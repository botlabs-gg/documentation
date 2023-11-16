# Image generation API (korra)

This api provides generated images (love-ship, waifu insult and more) for various purposes.

Codename: `korra`

## Generate Simple
### Endpoint `/auto-image/generate`
### Method `GET`

### Needed permissions

| Permission name | description                 |
|-----------------|-----------------------------|
| generate_simple | Used for simple generations |

### Query parameters

| Parameter name | Type               | description                                                       | default | required |
|----------------|--------------------|-------------------------------------------------------------------|---------|----------|
| type           | String             | type of the generation to create, possible types are listed below |         |     x    |
| face           | String (Hex Color) | only used with awooo type, defines color of face                  | fff0d3  |          |
| hair           | String (Hex Color) | only used with awooo type, defines color of hair/fur              | cc817c  |          |

### Types
### awooo:

 ![awoo image](https://i.imgur.com/9sJ21z3.png)

### eyes:

![eyes image](https://i.imgur.com/q9UL3fN.png)


### won:
![won image](https://i.imgur.com/hdapIsB.png?1)


## Discord Status
### Endpoint `/auto-image/discord-status`
### Method `GET`

### Needed permissions

| Permission name | description                 |
|-----------------|-----------------------------|
| generate_simple | Used for simple generations |

### Query parameters

| Parameter name | Type   | description                                                                          | default              | required |
|----------------|--------|--------------------------------------------------------------------------------------|----------------------|----------|
| status         | String | discord status of the mock, has to be one of the states listed below                 | online               |          |
| avatar         | String | uri encoded http/s url pointing to an avatar, has to have proper headers and be a direct link to an image | green default avatar |          |

### Example discord states

### online
![awoo online](https://i.imgur.com/jRtqgra.png)

### idle
![awoo idle](https://i.imgur.com/3kPwfLG.png)

### dnd
![awoo dnd](https://i.imgur.com/xnypBwF.png)

### streaming
![awoo streaming](https://i.imgur.com/FH9UQM1.png)

### offline
![awoo offline](https://i.imgur.com/2AOcxWZ.png)


## License generation


>Example payload


```json
{
	"title":"Spook License",
	"avatar":"https://imgur.com/zPn0DYT.png",
	"badges":[
		"https://imgur.com/zPn0DYT.png",
		"https://imgur.com/zPn0DYT.png",
		"https://imgur.com/zPn0DYT.png"
		],
	"widgets":["1", "2", "3"]
}
```


### Endpoint `/auto-image/license`

### Method `POST`

### Needed permissions

| Permission name  | description                  |
|------------------|------------------------------|
| generate_license | Used for generating licenses |

### Payload

| Name    | Type     | description                                                                                                     | default | required |
|---------|----------|-----------------------------------------------------------------------------------------------------------------|---------|----------|
| title   | String   | Title of the license                                                                                            |         |     x    |
| avatar  | String   | http/s url pointing to an image, has to have proper headers and be a direct link to an image                    |         |     x    |
| badges  | String[] | Array of http/s urls pointing to images, that should be used in the badges, same conditions as for avatar apply |         |          |
| widgets | String[] | Array of strings for filling the three boxes with text content                                                  |         |          |

### Example result

![example license](https://i.imgur.com/7ZZxrWf.png)


## Generate Waifuinsult

>Example payload

```json
{
  "avatar":"https://cdn.discordapp.com/avatars/121919449996460033/d52f23a57dbe54bb39b77d96d61a5a92.webp"
}
```

### Endpoint `/auto-image/waifu-insult`
### Method `POST`

### Needed permissions

| Permission name  | description                  |
|------------------|------------------------------|
| generate_waifu_insult | Used for generating waifuinsults |

### Payload

| Name   | Type   | description                                                                                  | default | required |
|--------|--------|----------------------------------------------------------------------------------------------|---------|----------|
| avatar | String | http/s url pointing to an image, has to have proper headers and be a direct link to an image |         |     x    |

### Example result

![Waifu Insult demo](https://i.imgur.com/R5VEQtS.png)


## Generate Loveship

>Example payload


```json
{
  "targetOne": "https://cdn.discordapp.com/avatars/185476724627210241/615ee9f0e97aab7fa0725165531df3a7.webp?size=256",
  "targetTwo": "https://cdn.discordapp.com/avatars/388799526103941121/b5acd5dd89aa8ff7c3600f2b7edaff57.webp?size=256"
}
```

### Endpoint `/auto-image/love-ship`
### Method `POST`

### Needed permissions

| Permission name  | description                  |
|------------------|------------------------------|
| generate_love_ship | Used for generating love ships |

### Payload

| Name   | Type   | description                                                                                  | default | required |
|--------|--------|----------------------------------------------------------------------------------------------|---------|----------|
| targetOne | String | http/s url pointing to an image, has to have proper headers and be a direct link to an image, image will be on the left side. |         |     x    |
| targetTwo | String | http/s url pointing to an image, has to have proper headers and be a direct link to an image, image will be on the right side. |         |     x    |

### Example result

![Love Ship demo](https://i.imgur.com/7Yw4Ls6.png)
