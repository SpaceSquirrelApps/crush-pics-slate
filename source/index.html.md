---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - json
  - shell

toc_footers:
  - <a href='#'>Sign Up for a API Key</a>

includes:
  - errors

search: true
---

# Introduction

> API Endpoint

```javascript
https://api.crush.pics
```

Welcome to the Crush.pics API. This API allows you to compress and optimize <code>JPEG</code>, <code>PNG</code>, <code>GIF</code> and <code>SVG</code> images.

To use Crush.pics API, you need to sign up for Crush.pics API services and obtain your <b>API key</b>. Once you have setup your Account, you can start using Crush.pics API.

# Authentication

> Example request

```shell
curl https://api.crush.pics/v1/compress \
-X POST
--form data='{ "api_key": "your-api-key", "image_url": "http://your-website.com/images/image.png" }'
```

```json
{
  "api_key": "your-api-key",
  "image_url": "http://your-website.com/images/image.png"
}

```

> To authenticate add api_key field to your requests:

Crush.pics API accepts HTTPS <code>POST</code> requests only. Each <code>POST</code> request needs to be a valid JSON object with mandatory field <code>api_key</code>

<aside class="notice">
Important: Crush.pics API supports only <code>HTTPS</code> protocol. All HTTP requests will be rejected.
</aside>

# Image compression

You can send any JPEG, PNG, GIF or SVG image to the Crush.pics API and it will automatically detect the type of image and compress it. You can choose to upload a file or provide a URL to the image.


`POST https://api.crush.pics/v1/compress`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
api_key |  | provide your unique API key
image | | provide image content in POST request
image_url |  | provide <code>URL</code> to the image you want to compress
lossy | true | lossy / lossless compression switch
quality | 80 | lossy compression quality settings (1-100)
callback_url | empty | With the callback_url the results will be sent in POST request to the Callback URL. If it's not set, results will be returned in the response body

## Image Upload

> Example Request

```shell
curl https://api.crush.pics/v1/compress \
-X POST \
--form data='{ "api_key": "your-api-key" }' \
--form upload=@unoptimized-image.jpg
```

```json
{
  "api_key": "your-api-key",
  "image": "DATA"
}
```
> Compress image by uploading file to API

`POST https://api.crush.pics/v1/compress`

Optimise image with direct upload in POST request

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
api_key |  | provide your unique API key
image | | provide image content in POST request

## Image URL

> Example request

```shell
curl https://api.crush.pics/v1/compress \
-X POST \
-H "Content-Type: application/json" \
--form data='{ "api_key": "your-api-key", "image_url": "http://your-website.com/images/image.png" }'
```

```json
{
  "api_key": "your-api-key",
  "image_url": "http://your-website.com/images/image.png"
}
```

> Compress image from URL

`POST https://api.crush.pics/v1/compress`

Optimise image by provide an URL to the image you want to optimise.

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
api_key |  | provide your unique API key
image_url |  | provide <code>URL</code> to the image you want to compress

<aside class="notice">
If HTTP Basic Authentication is enabled on your webserver simply include the username and password as part of the image URL like so: <code>https://username:password@my-website.com/images/image.png</code>
</aside>


# Response

> Example response

> HTTP/1.1 200 OK

```javascript
{
  "success": true,
  "crushed_url": "https://url-to-crushed-image.png",
  "original_file_size": 426781,
  "crushed_file_size": 203187,
  "file_saved_bytes": 223594,
  "original_width": 640,
  "original_height": 480,
  "image_type": "image/png"
}
```

Response body will have optimization results containing a success property, original file size, compressed file size, amount of savings, original image dimensions (in pixels) and optimized image URL.

# Callback URL


> Example request

```shell
curl https://api.crush.pics/v1/compress \
-X POST \
-H "Content-Type: application/json" \
--form data='{ "api_key": "your-api-key", "image_url": "http://your-website.com/images/image.png", "callback_url": "http://your-website.com/optimisation_results" }'
```

```json
{
  "api_key": "your-api-key",
  "image_url": "http://your-website.com/images/image.png",
  "callback_url": "http://your-website.com/optimisation_results"
}

```

> Example response

> HTTP/1.1 200 OK

```javascript
{
  "id": "ef23f35dbf2645f990ff224d50597d85"
}
```

> Response posted to the Callback URL once compression is complete:

> HTTP/1.1 200 OK

```javascript
{
  "success": true,
  "crushed_url": "https://url-to-crushed-image.png",
  "original_file_size": 426781,
  "crushed_file_size": 203187,
  "file_saved_bytes": 223594,
  "original_width": 640,
  "original_height": 480,
  "image_type": "image/png"
}
```

When you set <code>callback_url</code> parameter, unique image id will be returned in the response body and connection will be terminated immediately. Once compression is finished, Crush.pics API will send a POST request to the <code>callback_url</code> specified in your request. Image ID in the response will reflect the image ID in the results posted to your Callback URL.

# Image Resizing

> Example request

```shell
curl https://api.crush.pics/v1/compress \
-X POST \
-H "Content-Type: application/json" \
--form data='{ "api_key": "your-api-key", "image_url": "http://your-website.com/images/image.png", "resize": {"width": 320, "height": 100, "strategy": "crop"} }'
```

```json
{
  "api_key": "your-api-key",
  "image_url": "http://your-website.com/images/image.png",
  "resize": {
    "width": 320,
    "height": 100,
    "strategy": "crop"
  }
}

```

You can use Crush.pics API resizing option to create thumbnails or preview images for your applications. Crush.pics will first resize the given image and then optimize it. The resize option needs a few parameters to be passed, such as the desired width and/or height, as well as a mandatory strategy property.

# Image Conversion

> Example request

```shell
curl https://api.crush.pics/v1/compress \
-X POST \
-H "Content-Type: application/json" \
--form data='{ "api_key": "your-api-key", "image_url": "http://your-website.com/images/image.png","convert": {"format": "jpeg", "background": "#ffffff"} }'
```

```json
{
  "api_key": "your-api-key",
  "image_url": "http://your-website.com/images/image.png",
  "convert": {
    "format": "jpeg",
    "background": "#ffffff"
  }
}

```

If you want to convert image to different type/format, you can do it by passing <code>convert</code> object in your request JSON.

### Convert Parameters

Parameter | Default | Description
--------- | ------- | -----------
format |  | Desired image format you wish to convert your image into. This can accept one of the following values: <code>jpeg</code>, <code>png</code> or <code>gif</code>.
background | #ffffff | If you convert from transparent file formats (PNG or GIF) into fully opaque format like JPEG, you need to specify background color. The background property can be passed in HEX notation (for eg: <code>"#000000"</code>).

# Multiple Images

> Example request

```shell
curl https://api.crush.pics/v1/compress \
-X POST \
-H "Content-Type: application/json" \
--form data='{ "api_key": "your-api-key", "image_url": "http://your-website.com/images/image.png","convert": {"format": "jpeg", "background": "#ffffff"} }'
```

```json
{
  "api_key": "your-api-key",
  "image_url": "http://your-website.com/images/image.png",
  "resize": [
    {
      "id": "small",
      "width": "150",
      "height": "150"
    },
    {
      "id": "medium",
      "width": "300",
      "height": "300"
    },
    {
      "id": "large",
      "width": "600",
      "height": "600"
    },

  ]
}

```

> Example response:

> HTTP/1.1 200 OK

```javascript
{
  "success": true,

  "results": {
    "small": {
      "crushed_url": "https://url-to-crushed-image-small.png",
      "original_file_size": 426781,
      "crushed_file_size": 203187,
      "file_saved_bytes": 223594,
      "original_width": 640,
      "original_height": 480,
      "crushed_width": 150,
      "crushed_height": 150,
      "image_type": "image/png"
    },
    "medium": {
      "crushed_url": "https://url-to-crushed-image-medium.png",
      "original_file_size": 426781,
      "crushed_file_size": 203187,
      "file_saved_bytes": 223594,
      "original_width": 640,
      "original_height": 480,
      "crushed_width": 300,
      "crushed_height": 300,
      "image_type": "image/png"
    },
    "large": {
      "crushed_url": "https://url-to-crushed-image-large.png",
      "original_file_size": 426781,
      "crushed_file_size": 203187,
      "file_saved_bytes": 223594,
      "original_width": 640,
      "original_height": 480,
      "crushed_width": 600,
      "crushed_height": 600,
      "image_type": "image/png"
    }
  }
}

```

Crush.pics API allows you to upload a single image and get back up to ten separate sizes, incorporating different resizing strategies, within a single response.

To use multi-resize feature, you will need to add an array of objects as value to the resize param. Each object must contain an unique id, together with a strategy, and the parameters specific to that strategy. The id can either be a string or a number, bearing in mind that numbers will get coerced to a string for the response. On right, you can see example multi-resize object requesting three different sizes

## Resize object parameters

> Example request

```shell
curl https://api.crush.pics/v1/compress \
-X POST \
-H "Content-Type: application/json" \
--form data='{ "api_key": "your-api-key", "image_url": "http://your-website.com/images/image.png","convert": {"format": "jpeg", "background": "#ffffff"} }'
```

```json
{
  "api_key": "your-api-key",
  "image_url": "http://your-website.com/images/image.png",
  "resize": [
    {
      "id": "small",
      "width": "150",
      "height": "150",
      "quality": 65
    },
    {
      "id": "medium",
      "width": "300",
      "height": "300",
      "quality": 75
    },
    {
      "id": "large",
      "width": "600",
      "height": "600",
      "lossy": false
    },

  ]
}
```
By default, each resize object inherits certain values from the top level of the request - namely the values of the lossy, quality. However, those values can be overridden per resize object. For example, when requesting JPEG outputs you can specify different quality values per each output.

Below are several examples of overriding global request properties within resize objects:

# Preserve Metadata

> Example request

```shell
curl https://api.crush.pics/v1/compress \
-X POST \
-H "Content-Type: application/json" \
--form data='{ "api_key": "your-api-key", "image_url": "http://your-website.com/images/image.png", "preserve_meta": [ "orientation", "date", "copyright", "profile" ] }'
```

```json
{
  "api_key": "your-api-key",
  "image_url": "http://your-website.com/images/image.png",
  "preserve_meta": [ "orientation", "date", "copyright", "profile" ]
}
```

By default Crush.pics API will strip all the metadata found in an image to make the image file as small as it is possible, and in both lossy and lossless modes. Entries like EXIF, XMP and IPTC tags, colour profile information, etc. will be stripped altogether.

In order to preserve metadata, add an additional preserve_meta array to your JSON request with list of properties you want to preserve

# Image Orientation

> Example request

```shell
curl https://api.crush.pics/v1/compress \
-X POST \
-H "Content-Type: application/json" \
--form data='{ "api_key": "your-api-key", "image_url": "http://your-website.com/images/image.png", "auto_rotate": "true" }'
```

```json
{
  "api_key": "your-api-key",
  "image_url": "http://your-website.com/images/image.png",
  "auto_rotate": "true"
}
```

If you want auto-rotate images using EXIF orientation tags, add <code>"auto_rotate": true</code> to your request


