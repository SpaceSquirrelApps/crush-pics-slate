---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - json
  - shell

toc_footers:
  - <a href='#'>Sign Up for a API token</a>

includes:
  - webhooks
  - errors

search: true
---

# Introduction

> Base URL

```
https://api.crush.pics
```

### Authentication

To use Crush.pics API, you need to sign up for Crush.pics API services and obtain your <b>API token</b>. Once you have setup your Account, you can start using Crush.pics API.

### Supported formats

Crush.pics API support following formats:

- `JPEG`
- `PNG`
- `GIF`

### Supported compression types

Crush.pics API support following compression types:

- `lossless`
- `lossy` with ability to set up compression level
- `balanced`

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

# Account details

> Endpoint

```
https://api.crush.pics/v1/shop
```

> HTTP method

```
GET
```

## Response

> Example response

> HTTP/1.1 200 OK

```json
{
  "shop": {
    "login": "user@example.com",
    "email": null,
    "compression_type": "lossy",
    "compression_level_jpg": 85,
    "compression_level_png": 85,
    "compression_level_gif": null,
    "charged_at": null,
    "next_charge_at": null,
    "created_at": "2018-11-12T15:04:27Z",
    "updated_at": "2018-11-12T15:04:27Z",
    "plan_data": {
      "code": "free",
      "name": "Free",
      "price": "$0.00",
      "bytes": 25000000,
      "quota_usage": 3600688
    }
  }
}
```

# Update account settings

> Endpoint

```
https://api.crush.pics/v1/shop
```

> HTTP method

```
PATCH
```

> Example request

```shell
curl -X "POST" "https://api.crush.pics/v1/shop" \
     -H 'Accept: application/json' \
     -H 'Content-Type: application/json' \
     -d $'{
  "compression_level_png": 70,
  "compression_level_jpg": 70,
  "compression_level_gif": 70,
  "compression_type": "lossy",
  "callback_url": "https://your.site/crush_api/webhook",
}'
```

```json
{
  "compression_type": "lossy",
  "compression_level_jpg": 70,
  "compression_level_png": 70,
  "compression_level_gif": 70,
  "callback_url": "https://your.site/crush_api/webhook"
}
```

### Parameters

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
`compression_type` | string | no | `lossy` / `lossless` / `balanced` compression switch
`compression_level_jpg ` | integer | no | Lossy compression level for JPEG/JPG files
`compression_level_png ` | integer | no | Lossy compression level for PNG files
`compression_level_gif ` | integer | no | Lossy compression level for GIF files
`callback_url` | string | no | URL for receiving webhook events

## Response

> Example response

> HTTP/1.1 200 OK

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

> Example Request

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

### Parameters

Parameter | Type | Required | Default | Description
--------- | ---- | -------- | ------- | -----------
`file` | blob | yes | | Provide image content in POST request
`compression_type` | string | no | Fallback to account settings | `lossy` / `lossless` / `balanced` compression switch
`compression_level` | integer | no | Fallback to account settings | Lossy compression quality settings (1-100)
`resize` | array | no | [] | An array of styles to resize image

### Style object

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
`style` | string | yes | User-defined style name
`width` | integer | yes
`height` |integer | yes

## Image URL

Optimise image by provide an URL to the image you want to optimise.

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
  "compression_level": 65
}
```

### Parameters

Parameter | Type | Required | Default | Description
--------- | ---- | -------- | ------- | -----------
`image_url` | string | yes |  | Provide <code>URL</code> to the image you want to compress
`compression_type` | string | no | Fallback to account settings | `lossy` / `lossless` / `balanced` compression switc
`compression_level` | integer | no | Fallback to account settings | lossy compression quality settings (1-100)
`resize` | array | no | [] | An array of styles to resize image

### Style object

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
`style` | string | yes | User-defined style name
`width` | integer | yes
`height` |integer | yes

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

> Example Request

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

### Parameters

Parameter | Type | Required | Default | Description
--------- | ---- | -------- | ------- | -----------
`file` | blob | yes | | Provide image content in POST request
`compression_type` | string | no | Fallback to account settings | `lossy` / `lossless` / `balanced` compression switch
`compression_level` | integer | no | Fallback to account settings | Lossy compression quality settings (1-100)
`resize` | array | no | [] | An array of styles to resize image

### Style object

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
`style` | string | yes | User-defined style name
`width` | integer | yes
`height` |integer | yes

## Image URL

Optimise image by provide an URL to the image you want to optimise.

> Example request

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

### Parameters

Parameter | Type | Required | Default | Description
--------- | ---- | -------- | ------- | -----------
`image_url` | string | yes |  | Provide <code>URL</code> to the image you want to compress
`compression_type` | string | no | Fallback to account settings | `lossy` / `lossless` / `balanced` compression switc
`compression_level` | integer | no | Fallback to account settings | lossy compression quality settings (1-100)
`resize` | array | no | [] | An array of styles to resize image

### Style object

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
`style` | string | yes | User-defined style name
`width` | integer | yes
`height` |integer | yes

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

You can use Crush.pics API resizing option to create thumbnails or preview images for your applications. Crush.pics will first resize the given image and then optimize it. The resize option needs a few parameters to be passed, such as the desired width and/or height, as well as a mandatory style property.

<aside class="notice">
Crush.pics API does not support resizing only operations. All resizing operations perform on compressed images.
</aside>


# Dashboard

> Endpoint

```
https://api.crush.pics/v1/dashboard
```

> HTTP method

```
GET
```

## Response

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

# Image details

> Endpoint

```
https://api.crush.pics/v1/original_images/:id
```

> HTTP method

```
GET
```

### Query Parameters

Parameter | Type | Required | Description
--------- | ---- |-------- | -----------
`id` | integer | yes | Image ID

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

# List images

> Endpoint

```
https://api.crush.pics/v1/original_images?page=1
```

> HTTP method

```
GET
```

### Query Parameters

Parameter | Type | Required | Default | Description
--------- | ---- | -------- | ------- | -----------
`page` | integer | no | 1 | Page number

## Response

> Example response

> HTTP/1.1 200 OK

```json
{
  "original_images": [
    {
      "id": 4523,
      "compression_type": "lossy",
      "compression_level": 85,
      "origin": "file",
      "status": "completed",
      "created_at": "2019-07-25T15:47:07.836Z",
      "updated_at": "2019-07-26T03:48:00.201Z"
    },
    {
      "id": 4514,
      "compression_type": "lossy",
      "compression_level": 85,
      "origin": "file",
      "status": "completed",
      "created_at": "2019-07-24T09:47:54.942Z",
      "updated_at": "2019-07-24T21:48:00.541Z"
    },
    {
      "id": 4488,
      "compression_type": "balanced",
      "compression_level": 85,
      "origin": "shopify",
      "status": "completed",
      "created_at": "2019-07-19T14:22:07.610Z",
      "updated_at": "2019-07-20T02:23:00.149Z"
    },
    {
      "id": 4487,
      "compression_type": "balanced",
      "compression_level": 85,
      "origin": "shopify",
      "status": "completed",
      "created_at": "2019-07-19T14:21:50.287Z",
      "updated_at": "2019-07-20T02:22:00.478Z"
    },
    {
      "id": 4486,
      "compression_type": "balanced",
      "compression_level": 85,
      "origin": "shopify",
      "status": "completed",
      "created_at": "2019-07-19T14:21:13.540Z",
      "updated_at": "2019-07-20T02:22:00.515Z"
    },
    {
      "id": 4485,
      "compression_type": "balanced",
      "compression_level": 85,
      "origin": "shopify",
      "status": "completed",
      "created_at": "2019-07-19T14:21:11.969Z",
      "updated_at": "2019-07-20T02:22:00.483Z"
    },
    {
      "id": 4484,
      "compression_type": "balanced",
      "compression_level": 85,
      "origin": "shopify",
      "status": "completed",
      "created_at": "2019-07-19T14:21:09.659Z",
      "updated_at": "2019-07-20T02:22:00.495Z"
    },
    {
      "id": 4483,
      "compression_type": "balanced",
      "compression_level": 85,
      "origin": "shopify",
      "status": "completed",
      "created_at": "2019-07-19T14:20:54.646Z",
      "updated_at": "2019-07-20T02:21:00.402Z"
    },
    {
      "id": 4160,
      "compression_type": "balanced",
      "compression_level": 85,
      "origin": "shopify",
      "status": "enqueued",
      "created_at": "2019-07-17T12:17:44.097Z",
      "updated_at": "2019-07-18T00:18:00.273Z"
    },
    {
      "id": 4016,
      "compression_type": null,
      "compression_level": null,
      "origin": "file",
      "status": "completed",
      "created_at": "2019-06-27T19:57:33.628Z",
      "updated_at": "2019-06-28T07:58:00.206Z"
    }
  ],
  "pagination": {
    "current": 1,
    "total_pages": 13,
    "total_count": 124,
    "per_page": 10,
    "count": 10
  }
}
```