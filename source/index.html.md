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

> Endpoint

```
https://api.crush.pics/v1/compress
```

> HTTP method

```
POST
```

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

> Endpoint

```
https://api.crush.pics/v1/compress
```

> HTTP method

```
POST
```

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
compression_type | no | fallback to account settings | `lossy` / `lossless` / `balanced` compression switch
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
compression_type | no | fallback to account settings | `lossy` / `lossless` / `balanced` compression switch
compression_level | no | fallback to account settings | lossy compression quality settings (1-100)
resize | no | [] | an array of styles to resize image. Each object contains `style` (string, user-defined style name), `strategy` (string), `width` (integer) and `height` (integer) attributes

## Response

> Example response

> HTTP/1.1 200 OK

```json
{
  "original_image": {
    "id": 5,
    "compression_type": "lossy",
    "compression_level": 65,
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

> Endpoint

```
https://api.crush.pics/v1/original_images
```

> HTTP method

```
POST
```

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
    "compression_type": "lossy",
    "compression_level": 65,
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
curl -X "POST" "https://api.crush.pics/v1/original_images" \
     -H 'Accept: application/json' \
     -H 'Content-Type: multipart/form-data; boundary=ArRCoq7' \
     -H 'Authorization: Bearer your-api-token' \
     -d $'{
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
      "style": "thumb"
    },
    {
      "width": 80,
      "height": 25,
      "style": "pico"
    }
  ]
}

```

You can use Crush.pics API resizing option to create thumbnails or preview images for your applications. Crush.pics will first resize the given image and then optimize it. The resize option needs a few parameters to be passed, such as the desired width and/or height, as well as a mandatory strategy property.

**Note**: Crush.pics API does not support resizing only. All resizing operations perform on compressed images.


# Dashboard

> Endpoint

```
https://api.crush.pics/v1/dashboard
```

> HTTP method

```
GET
```

> Example response

> HTTP/1.1 200 OK

```json
{
  "stats": {
    "original_images_count": 475,
    "optimized_images_count": 475,
    "consumption": {
      "type": "six_months",
      "data": [
        {
          "origin": "file",
          "size": null,
          "period": 2
        },
        {
          "origin": "shopify_article",
          "size": null,
          "period": 2
        },
        {
          "origin": "file",
          "size": null,
          "period": 3
        },
        {
          "origin": "file",
          "size": null,
          "period": 6
        },
        {
          "origin": "shopify",
          "size": 1858202,
          "period": 7
        },
        {
          "origin": "file",
          "size": null,
          "period": 7
        }
      ]
    }
  }
}
```

