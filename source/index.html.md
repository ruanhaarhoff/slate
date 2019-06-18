---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Capsured API! You can use this API to access Capsured API endpoints, which can get information on various users, items, and their insurance in our database.

We have language bindings in Shell, maybe more to come soon! You can view code examples in the dark area to the right.

# Authentication

The API uses **basic authentication** to access the endpoints. Two middleware are setup, including UserAuthenticate and DeviceAuthenticate.

## UserAuthenticate

The UserAuthenticate middleware checks for the correct login details from the client, including their email & password.
This request is used to authenticate the user on initial login. The app will keep the user authenticated
using the <code>access_token</code> that is returned from the login request, using the DeviceAuthenticate middleware.

The user has to be authenticated with UserAuthenticate first, before his session can be kept by DeviceAuthenticate. 

### HTTP Request

`POST https://app.capsured.co.za/api/user/login`

> EXAMPLE REQUEST

```shell
# The email and password should be base64 encoded and specified in the header through basic auth
curl https://app.capsured.co.za/api/user/login \
  --request POST \
  -H "Content-Type: application/json" \
  -H "Authorization: Basic email:password" \
  -d '{
    "device_id": "05B805D4-1454-4AE3-9F94-C290AB708137",
    "type": "ios",
    "version": "12.3.1",
  }'
```

#### BODY PARAMETERS

Parameter | Description      
--------- |  -----------
device_id |  *string*. The device unique id. 
type      |  *string*. The type of device - Android, IOS, etc.
version   |  *string*. Version of the device

> Make sure to replace `email:password` with the base64 encoded email and password.

> EXAMPLE RESPONSE

```json
    {
        "success": true,
        "errorCode": 0,
        "msg": "",
        "info": "",
        "data": {
            "user": {
                "id": 1,
                "first_name": "Ruan",
                "last_name": "Haarhoff",
                "email": "ruan.haarhoff@gmail.com",
                "phone": null,
                "id_number": "9408085029086",
                "id_country": "ZA",
                "created_at": "2019-04-11 12:17:31",
                "updated_at": "2019-04-12 08:15:21",
                "deleted_at": null,
                "area_code": "7530",
                "age": 24
            },
            "device_id": "05B805D4-1454-4AE3-9F94-C290AB708137",
            "access_token": "cb816b0a-b8c3-4919-bc6a-f3f33cf7c2c9"
        }
    }
```

<aside class="notice">
Replace <code>email:password</code> with the base64 encoded email & password.
</aside>

## DeviceAuthenticate

The app will keep the user authenticated using the <code>access_token</code> that is returned from the `/api/user/login` request.
This authentication will happen on all requests that required the user login. Thus, for most requests following,
the Authorization header will need to be set with the base64 encoded `device_id` and `acces_token` of the user. An
example request is shown below.
 

### HTTP Request

`POST https://app.capsured.co.za/api/<endpoint>`

> EXAMPLE REQUEST

```shell
# The device_id and access_token should be base64 encoded and specified in the header through basic auth
curl https://app.capsured.co.za/api/user/login \
  --request POST \
  -H "Content-Type: application/json" \
  -H "Authorization: Basic device_id:access_token" \
  -d '{
    "key": "value"
  }'
```

> Make sure to replace `device_id:access_token` with the base64 encoded device_id and access_token.

# Users

## Register a User

Register a new user and their Device.

### HTTP Request

`POST https://app.capsured.co.za/api/register`

> EXAMPLE REQUEST

```shell
curl https://app.capsured.co.za/api/user/register \
  --request GET \
  -H "Content-Type: application/json" 
  -b '{
    "user": {
  		"first_name": "Ruan",
  		"last_name": "Haarhoff",
  		"email": "rhaarhoff@4i.co.za",
  		"password": "MeowMeowLikeACow",
  		"password_confirmation": "MeowMeowLikeACow",
  		"phone": "0846046255",
  		"id_number": "9408085029086",
  		"area_code": "7530"
    },
    "device": {
    	"device_id": "05B805D4-1454-4AE3-9F94-C290AB708137",
    	"type": "ios",
    	"version": "12.3.1"
    }
  }'
```

#### BODY PARAMETERS

Parameter | Description      
--------- |  -----------
first_name |  *string*. The first name of the user. 
last_name  |  *string*. The last name of the user.
email      |  *string*. The email of the user.
password      |  *string*. The password of the user.
password_confirmation      |  *string*. The confirmed password of the user (should be the same as `password`.
phone <sub>*optional*</sub>   |  *string*. Phone number of the user (should be a South African number).
area_code  |  *string*. Area code where the user lives.
device_id |  *string*. The device unique id. 
type      |  *string*. The type of device - Android, IOS, etc.
version   |  *string*. Version of the device

> EXAMPLE RESPONSE

```json
{
    "success": true,
    "errorCode": 0,
    "msg": "",
    "info": "",
    "data": {
        "user": {
            "id": 1,
            "first_name": "Ruan",
            "last_name": "Haarhoff",
            "email": "ruan.haarhoff@gmail.com",
            "phone": null,
            "id_number": "9408085029086",
            "id_country": "ZA",
            "created_at": "2019-04-11 12:17:31",
            "updated_at": "2019-04-12 08:15:21",
            "deleted_at": null,
            "area_code": "7530",
            "age": 24
        },
        "device_id": "05B805D4-1454-4AE3-9F94-C290AB708137",
        "access_token": "cb816b0a-b8c3-4919-bc6a-f3f33cf7c2c9"
    }
}
```

## Get a User

Get the authenticated user and their card details.

### HTTP Request

`GET https://app.capsured.co.za/api/user`

> EXAMPLE REQUEST

```shell
curl https://app.capsured.co.za/api/user \
  --request GET \
  -H "Content-Type: application/json" \
  -H "Authorization: Basic device_id:access_token" \
```

> EXAMPLE RESPONSE

```json
{
    "success": true,
    "errorCode": 0,
    "msg": "",
    "info": "",
    "data": {
        "user": {
            "id": 1,
            "first_name": "Ruan",
            "last_name": "Haarhoff",
            "email": "ruan.haarhoff@gmail.com",
            "phone": null,
            "id_number": "9408085029086",
            "id_country": "ZA",
            "created_at": "2019-04-11 12:17:31",
            "updated_at": "2019-04-12 08:15:21",
            "deleted_at": null,
            "area_code": "7530",
            "age": 24
        },
        "cards": [
            {
                "bin": "403822",
                "holder": "Ruan Haarhoff",
                "brand": "VISA",
                "expiry_month": "08",
                "expiry_year": "2019",
                "last_4_digits": "0011",
                "id": "2393ac6a-aa2e-4e1a-b240-ff62403b02d8"
            }
        ]
    }
}
```
<aside class="warning">
This request requires the device authentication in the header.
</aside>

## Edit a User

Edit / Update the authenticated user.

### HTTP Request

`PUT https://app.capsured.co.za/api/user/edit`

> EXAMPLE REQUEST

```shell
curl https://app.capsured.co.za/api/user \
  --request GET \
  -H "Content-Type: application/json" \
  -H "Authorization: Basic device_id:access_token" \
  -b '{
    "first_name": "Ruan",
    "last_name": "Haarhofff",
    "phone": "0846046252",
    "area_code": "4020"
    "id_number" : "9408085029086"
  }'
```

#### BODY PARAMETERS

Parameter | Description      
--------- |  -----------
first_name <sub>*optional*</sub> |  *string*. The first name of the user. 
last_name <sub>*optional*</sub>      |  *string*. The last name of the user.
phone <sub>*optional*</sub>   |  *string*. Phone number of the user (should be a South African number).
area_code <sub>*optional*</sub>   |  *string*. Area code where the user lives.
id_number <sub>*optional*</sub>   |  *string*. South African ID number of the user.

> EXAMPLE RESPONSE

```json
{
    "success": true,
    "errorCode": 0,
    "msg": "",
    "info": "",
    "data": {
        "user": {
            "id": 1,
            "first_name": "Ruan",
            "last_name": "Haarhoff",
            "email": "ruan.haarhoff@gmail.com",
            "phone": null,
            "id_number": "9408085029086",
            "id_country": "ZA",
            "created_at": "2019-04-11 12:17:31",
            "updated_at": "2019-04-12 08:15:21",
            "deleted_at": null,
            "area_code": "7530",
            "age": 24
        },
        "cards": [
            {
                "bin": "403822",
                "holder": "Ruan Haarhoff",
                "brand": "VISA",
                "expiry_month": "08",
                "expiry_year": "2019",
                "last_4_digits": "0011",
                "id": "2393ac6a-aa2e-4e1a-b240-ff62403b02d8"
            }
        ]
    }
}
```
<aside class="warning">
This request requires the device authentication in the header.
</aside>

# Items

## Add an Item

Add an item to the authenticated user. Since this call will be receiving an image, the content type would need
to be changed to `multipart/form-data`.

### HTTP Request

`POST https://app.capsured.co.za/api/item/add`

> EXAMPLE REQUEST

```shell
curl https://app.capsured.co.za/api/item/add \
  --request POST \
  -H "Content-Type: multipart/form-data" \
  -H "Authorization: Basic device_id:access_token" \
  -F 'name=My Canon' \
  -F key=camera_nikon_9 \
  -F product_type=camera \
  -F make=Nikon \
  -F model=4000D \
  -F serial_number=1234567890 \
  -F 'profile=@/path/to/file/example_file.png' \
  -F purchase_date=2018-12-03
```

#### FORM PARAMETERS

Parameter | Description      
--------- |  -----------
name  | *string* Name of the item.
key | *string* The **Root** specific `key` for this item.
product_type | *string* The **Root** specific `product_type` for this item.
make | *string* The **Root** specific `make` for this item.
model | *string* The **Root** specific `model` for this item.
serial_number | *string* The serial number of the item.
purchase_date | *string* The date the item was purchased. 
profile | *file* The profile picture of the item.

> EXAMPLE RESPONSE

```json
{
    "success": true,
    "errorCode": 0,
    "msg": "",
    "info": "",
    "data": {
        "user": {
            "id": 1,
            "first_name": "Ruan",
            "last_name": "Haarhoff",
            "email": "ruan.haarhoff@gmail.com",
            "phone": null,
            "id_number": "9408085029086",
            "id_country": "ZA",
            "created_at": "2019-04-11 12:17:31",
            "updated_at": "2019-04-12 08:15:21",
            "deleted_at": null,
            "area_code": "7530",
            "age": 24
        },
        "cards": [
            {
                "bin": "403822",
                "holder": "Ruan Haarhoff",
                "brand": "VISA",
                "expiry_month": "08",
                "expiry_year": "2019",
                "last_4_digits": "0011",
                "id": "2393ac6a-aa2e-4e1a-b240-ff62403b02d8"
            }
        ]
    }
}
```
<aside class="warning">
This request requires the device authentication in the header.
</aside>

## Get a Quote for an Item

Get a quote for an item for an authenticated user. This makes a direct call to the *Root*
api endpoint, with minimal additions to the request from our backend. There are two different types
of quotes which can be specified using the `cover_type` key. They are `on_demand` and `monthly`.

The Quotes get a bit complicated when a user is starting to get quotes for multiple items. This is 
because all items are loaded onto the same `Insurance Policy`. This means that when a user gets a quote for
a new item (with other items already on the policy), the get has to take all of this into account. The request response
will, however, look something like below.

### On Demand Quote  

### HTTP Request

`POST https://app.capsured.co.za/api/quote`

> EXAMPLE REQUEST

```shell
curl https://app.capsured.co.za/api/item/add \
  --request POST \
  -H "Content-Type: multipart/form-data" \
  -H "Authorization: Basic device_id:access_token" \
  -b '{
  "covered_items": [
    {
      "product_type": "camera",
      "cover_type": "on_demand",
      "model": "camera_nikon_9",
      "serial_number": "1234567890",
      "purchase_date": "2018-01-01",
      "retail_value": 100000,
      "benefit_type": "standard",
      "underwater_usage": true,
      "perils": {
          "accidental_damage": true,
          "theft": true
      },
      "excess": {
          "type": "fixed",
          "value": 50000
      },
      "usage": "personal",
      "outside_home": true,
      "overnight_storage": "locked_safe",
      "daytime_storage": "locked_safe",
      "protective_gear": null,
      "extreme_sports": null,
      "existing_damage": false,
      "cover_periods": [
        {
          "start_date": "2019-07-01",
          "end_date": "2019-07-20"
        }
      ]
    }
  ]
}'
```

#### BODY PARAMETERS

I am not going to duplicate the ROOT documentation. Visit their docs to see what everything is and how it works.

> EXAMPLE RESPONSE

```json
{
    "success": true,
    "errorCode": 0,
    "msg": "",
    "info": "",
    "data": {
        "quote": [
            {
                "covered_item_id": "ca93aab8-72c0-4400-bd3e-037ad0ee6d12",
                "created_at": "2019-06-18T10:56:41.921Z",
                "created_by": {
                    "type": "api_key",
                    "id": "3e607a80-11c1-45f6-8215-f82a876b0ec6",
                    "owner_id": "0e733cb3-c8e3-413b-8b06-47c3c6918b3d"
                },
                "cover_type": "on_demand",
                "monthly_premium": 0,
                "module": {
                    "product_type": "camera",
                    "model": "camera_nikon_9",
                    "serial_number": "1234567890",
                    "purchase_date": "2018-01-01",
                    "retail_value": 260000,
                    "benefit_type": "standard",
                    "underwater_usage": true,
                    "perils": {
                        "accidental_damage": true,
                        "theft": true
                    },
                    "excess": {
                        "type": "fixed",
                        "value": 50000
                    },
                    "usage": "personal",
                    "outside_home": true,
                    "overnight_storage": "locked_safe",
                    "daytime_storage": "locked_safe",
                    "protective_gear": null,
                    "extreme_sports": null,
                    "existing_damage": false,
                    "sum_assured": 260000,
                    "model_pretty_name": "Nikon D850 DSLR Camera (Body Only)"
                },
                "cover_periods": [
                    {
                        "cover_period_id": "185c136a-301d-4fb1-8117-d579a76f8c95",
                        "start_date": "2019-07-05",
                        "end_date": "2019-07-20",
                        "premium": 1099
                    }
                ]
            }
        ]
    }
}
```
<aside class="warning">
This request requires the device authentication in the header.
</aside>

### Monthly Quote  

### HTTP Request

`POST https://app.capsured.co.za/api/quote`

> EXAMPLE REQUEST

```shell
curl https://app.capsured.co.za/api/item/add \
  --request POST \
  -H "Content-Type: multipart/form-data" \
  -H "Authorization: Basic device_id:access_token" \
  -b '{
    "covered_items": [
      {
        "cover_type": "monthly",
        "perils": {
          "theft": true,
          "accidental_damage": true
        },
        "usage": "personal",
        "overnight_storage": "not_applicable",
        "daytime_storage": "not_applicable",
        "outside_home": false,
        "underwater_usage": true,
        "borrow_or_lend": true,
        "benefit_type": "standard",
        "hobby_club": false,
        "excess": {
          "type": "fixed",
          "value": 150000
        },
        "protective_gear": null,
        "extreme_sports": null,
        "existing_damage": false,
        "retail_value": 1234500,
        "product_type": "camera",
        "model": "camera_nikon_9",
        "serial_number": "1234567890",
        "purchase_date": "2019-03-06 00:00:00"
      }
    ]
  }'
```

#### BODY PARAMETERS

I am not going to duplicate the ROOT documentation. Visit their docs to see what everything is and how it works.

> EXAMPLE RESPONSE

```json
{
    "success": true,
    "errorCode": 0,
    "msg": "",
    "info": "",
    "data": {
        "quote": [
            {
                "covered_item_id": "70dbc3b0-3c80-4034-b579-08a402974bc8",
                "created_at": "2019-06-18T11:10:32.883Z",
                "created_by": {
                    "type": "api_key",
                    "id": "3e607a80-11c1-45f6-8215-f82a876b0ec6",
                    "owner_id": "0e733cb3-c8e3-413b-8b06-47c3c6918b3d"
                },
                "cover_type": "monthly",
                "monthly_premium": 8634,
                "module": {
                    "perils": {
                        "theft": true,
                        "accidental_damage": true
                    },
                    "usage": "personal",
                    "overnight_storage": "not_applicable",
                    "daytime_storage": "not_applicable",
                    "outside_home": false,
                    "underwater_usage": true,
                    "borrow_or_lend": true,
                    "benefit_type": "standard",
                    "hobby_club": false,
                    "excess": {
                        "type": "fixed",
                        "value": 150000
                    },
                    "protective_gear": null,
                    "extreme_sports": null,
                    "existing_damage": false,
                    "retail_value": 1234500,
                    "product_type": "camera",
                    "model": "camera_nikon_9",
                    "serial_number": "1234567890",
                    "purchase_date": "2019-03-06 00:00:00",
                    "sum_assured": 1234500,
                    "model_pretty_name": "Nikon D850 DSLR Camera (Body Only)"
                },
                "cover_periods": null
            }
        ]
    }
}
```
<aside class="warning">
This request requires the device authentication in the header.
</aside>
