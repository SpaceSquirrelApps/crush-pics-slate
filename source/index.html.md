---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - json
  - shell

toc_footers:
  - <a href='#'>Sign Up for a API token</a>

includes:
  - errors

search: true
---

# Introduction

> API Endpoint

```
https://api.crush.pics
```

Welcome to the Crush.pics API. This API allows you to compress and optimize <code>JPEG</code>, <code>PNG</code>, <code>GIF</code> and <code>SVG</code> images.

To use Crush.pics API, you need to sign up for Crush.pics API services and obtain your <b>API token</b>. Once you have setup your Account, you can start using Crush.pics API.

# Authentication

> Example request

```shell
curl -X "POST" "https://api.crush.pics/v1/compress" \
     -H 'Accept: application/json' \
     -H 'Content-Type: application/json' \
     -H 'Authorization: Bearer your-api-token' \
     -F "image_url=https://your-website.com/images/image.png" \
     -F "compression_type=lossy" \
     -F "compression_level=65"
```

```json
{
  "image_url": "http://your-website.com/images/image.png",
  "compression_type": "lossy",
  "compression_level": "65"
}
```

Crush.pics API accepts HTTPS <code>POST</code>, <code>PATCH</code>, <code>GET</code> requests. Each request needs to be a valid JSON object with mandatory headers <code>Authorization</code>, <code>Accept</code> and <code>Content-Type</code>.

<aside class="notice">
Important: Crush.pics API supports only <code>HTTPS</code> protocol. All HTTP requests will be rejected.
</aside>

# Image compression (Synchronously)

You can send any JPEG, PNG, GIF or SVG image to the Crush.pics API and it will automatically detect the type of image and compress it. You can choose to upload a file or provide a URL to the image.

`POST https://api.crush.pics/v1/compress`

## Image Upload

> Example Request - compress image by uploading file

```shell
curl -X "POST" "https://api.crush.pics/v1/compress" \
     -H 'Accept: application/json' \
     -H 'Content-Type: multipart/form-data; boundary=Fu0hjwe' \
     -H 'Authorization: Bearer your-api-token' \
     -F "file=@unoptimized-image.jpg" \
     -F "compression_type=lossy" \
     -F "compression_level=65" \
```

```json
{
  "file": "@unoptimized-image.jpg",
  "compression_type": "lossy",
  "compression_level": 65
}
```

### Query Parameters

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
file | yes | | provide image content in POST request
compression_type | no | fallback to account settings | lossy / lossless compression switch
compression_level | no | fallback to account settings | lossy compression quality settings (1-100)
resize | no | [] | an array of styles to resize image. Each object contains `style` (string, user-defined style name), `strategy` (string), `width` (integer) and `height` (integer) attributes

## Image URL

Optimise image by provide an URL to the image you want to optimise.

> Example request - compress image from URL

```shell
curl -X "POST" "https://api.crush.pics/v1/compress" \
     -H 'Accept: application/json' \
     -H 'Content-Type: application/json' \
     -H 'Authorization: Bearer your-api-token' \
     -F "image_url=https://your-website.com/images/image.png" \
     -F "compression_type=lossy" \
     -F "compression_level=65"
```

```json
{
  "image_url": "http://your-website.com/images/image.png",
  "compression_type": "lossy",
  "compression_level": 65
}
```

### Query Parameters

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
image_url | yes |  | provide <code>URL</code> to the image you want to compress
compression_type | no | fallback to account settings | lossy / lossless compression switch
compression_level | no | fallback to account settings | lossy compression quality settings (1-100)
resize | no | [] | an array of styles to resize image. Each object contains `style` (string, user-defined style name), `strategy` (string), `width` (integer) and `height` (integer) attributes

## Response

> Example response

> HTTP/1.1 200 OK

```json
{
  "original_image": {
    "id": 5,
    "origin": "file",
    "size": 297412,
    "file_type": "image/png",
    "filename": "photo.png",
    "status": "completed",
    "created_at": "2018-07-09T13:07:42.943Z",
    "updated_at": "2018-07-09T13:07:46.074Z",
    "link": "https://api.crush.pics/v1/files/X8KxL3cD1viTBasepBwSL4Tz/photo.png",
    "optimized_images": [
      {
        "id": 4,
        "status": "enqueued",
        "compression_type": "lossless",
        "compression_level": 0,
        "style": "icon",
        "width": 150,
        "height": 150,
        "link": "https://api.crush.pics/v1/files/ZdcSBAk1m4XMeH9KNvSmWFNN/photo.png",
        "size": 465,
        "file_type": "image/png",
        "filename": "photo.png",
        "created_at": "2018-07-09T13:07:43.005Z",
        "updated_at": "2018-07-09T13:07:46.069Z"
      }
    ]
  }
}
```

Response body will have optimization results containing a success property, original file size, compressed file size, amount of savings, original image dimensions (in pixels) and optimized image URL.

# Image compression (Asynchronously, respond with webhooks)

You can send any JPEG, PNG, GIF or SVG image to the Crush.pics API and it will automatically detect the type of image and compress it. You can choose to upload a file or provide a URL to the image.

`POST https://api.crush.pics/v1/original_images`

## Image Upload

Optimise image with direct upload in POST request

> Example Request - compress image by uploading file

```shell
curl -X "POST" "https://api.crush.pics/v1/original_images" \
     -H 'Accept: application/json' \
     -H 'Content-Type: multipart/form-data; boundary=Fu0hjwe' \
     -H 'Authorization: Bearer your-api-token' \
     -F "file=@unoptimized-image.jpg" \
     -F "compression_type=lossy" \
     -F "compression_level=65" \
```

```json
{
  "file": "@unoptimized-image.jpg",
  "compression_type": "lossy",
  "compression_level": 65
}
```

### Query Parameters

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
file | yes | | provide image content in POST request
compression_type | no | fallback to account settings | lossy / lossless compression switch
compression_level | no | fallback to account settings | lossy compression quality settings (1-100)
resize | no | [] | an array of styles to resize image. Each object contains `style` (string, user-defined style name), `strategy` (string), `width` (integer) and `height` (integer) attributes

## Image URL

Optimise image by provide an URL to the image you want to optimise.

> Example request - compress image from URL

```shell
curl -X "POST" "https://api.crush.pics/v1/original_images" \
     -H 'Accept: application/json' \
     -H 'Content-Type: application/json' \
     -H 'Authorization: Bearer your-api-token' \
     -F "image_url=https://your-website.com/images/image.png" \
     -F "compression_type=lossy" \
     -F "compression_level=65"
```

```json
{
  "image_url": "http://your-website.com/images/image.png",
  "compression_type": "lossy",
  "compression_level": 65
}
```

### Query Parameters

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
image_url | yes |  | provide <code>URL</code> to the image you want to compress
compression_type | no | fallback to account settings | lossy / lossless compression switch
compression_level | no | fallback to account settings | lossy compression quality settings (1-100)
resize | no | [] | an array of styles to resize image. Each object contains `style` (string, user-defined style name), `strategy` (string), `width` (integer) and `height` (integer) attributes

<aside class="notice">
If HTTP Basic Authentication is enabled on your webserver simply include the username and password as part of the image URL like so: <code>https://username:password@my-website.com/images/image.png</code>
</aside>

## Response

> Example response

> HTTP/1.1 200 OK

```json
{
  "original_image": {
    "id": 5,
    "origin": "file",
    "size": 297412,
    "file_type": "image/png",
    "filename": "photo.png",
    "status": "enqueued",
    "created_at": "2018-06-29T15:52:22.589Z",
    "updated_at": "2018-06-29T15:52:22.596Z",
    "link": "https://api.crush.pics/v1/files/LX8Xhv3Z2T2dQDSRnrC3UBxg/photo.png"
  }
}
```

Response body will contain a set of basic image attributes e.g `id`, `size`, `filename`, `status` and etc. Using the image `id` you can fetch results of image optimization and get compressed/resized images.

# Image Resizing

> Example request

```shell
curl -X "POST" "https://api.crush.pics/v1/compress" \
     -H 'Accept: application/json' \
     -H 'Content-Type: multipart/form-data; boundary=ArRCoq7' \
     -H 'Authorization: Bearer your-api-token' \
     -d $'{
  "origin": "file",
  "compression_type": "lossy",
  "resize[][width]": "100",
  "compression_level": "65",
  "image_url": "http://your-website.com/images/image.png",
  "resize[][style]": "thumb",
  "resize[][height]": "100",
  "resize[][width]": "320",
  "resize[][height]": "crop",
  "resize[][style]": "pico",
  "resize[][height]": "25",
  "resize[][width]": "80",
  "resize[][height]": "crop",
}'
```

```json
{
  "image_url": "http://your-website.com/images/image.png",
  "resize": [
    {
      "width": 320,
      "height": 100,
      "strategy": "crop",
      "style": "thumb"
    },
    {
      "width": 80,
      "height": 25,
      "strategy": "crop",
      "style": "pico"
    }
  ]
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
      "crushed_url": "https://api.crush.pics/download/3e2708872ead4dbdb7e911721a0bd420.png",
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
      "crushed_url": "https://api.crush.pics/download/ba34f15cd34543fcbbe62c13595a086f.png",
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
      "crushed_url": "https://api.crush.pics/download/19805cbee6c543a78514a2e4424964e4.png",
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
--form data='{ "api_key": "your-api-key", "image_url": "http://your-website.com/images/image.png",   "resize": [{"id": "small", "width": "150", "height": "150", "quality": 65 }, {"id": "medium", "width": "300", "height": "300", "quality": 75 }, {"id": "large", "width": "600", "height": "600", "lossy": false } ] }'
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
    }
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
