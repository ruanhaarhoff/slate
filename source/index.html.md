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

`POST https://app.capsured.co.za/api/user/register`

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
        "device": {
            "id": 1
            "device_id": "05B805D4-1454-4AE3-9F94-C290AB708137",
            "type": "ios",
            "user_id": 1,
            "access_token": "3f62e1e7-d248-405f-bdeb-b223725aa54f"
        },
        "access_token": "3f62e1e7-d248-405f-bdeb-b223725aa54f"
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
                "bin": "403844",
                "holder": "Ruan Haarhoff",
                "brand": "VISA",
                "expiry_month": "08",
                "expiry_year": "2019",
                "last_4_digits": "0022",
                "id": "2453ac6a-aa2e-4e1a-c340-ff62403b0448"
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
        "item": {
            "id": 1,
            "name": "My Canon",
            "key": "camera_nikon_9",
            "product_type": "camera",
            "make": "Nikon",
            "model": "4000D",
            "serial_number": "1234567890",
            "purchase_date": "2018-12-03",
            "status": 1,
            "updated_at": "2019-06-27 11:17:06",
            "created_at": "2019-06-27 11:17:06",
            "files": [
                {
                    "id": 1,
                    "path": "local/users/1/items/1/Sk841BcyQ7NhrmZysrX9d3Q89d7Y2xGSmc9bzn1t.png",
                    "file_name": "test.png",
                    "type": "profile"
                }
            ]
        }
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
a new item (with other items already on the policy), the get has to take all of this into account. Thus, the quote
response will look a bit different when the user has no policy (meaning he has not activate any items yet). If a user
has no policy, the response will contain a `quote_package_id`, which will be used when activating the first item.

### On Demand Quote - No Policy (No items activated yet)

### HTTP Request

`POST https://app.capsured.co.za/api/quote`

> EXAMPLE REQUEST

```shell
curl https://app.capsured.co.za/api/quote \
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
                "quote_package_id": "cb0b50e1-128f-4146-8804-17d1bcafa6cb",
                "package_name": "Device Cover",
                "sum_assured": 260000,
                "base_premium": 0,
                "suggested_premium": 0,
                "module": {
                    "type": "capsured",
                    "covered_items": [
                        {
                            "cover_type": "on_demand",
                            "monthly_premium": 0,
                            "cover_periods": [
                                {
                                    "start_date": "2019-07-05",
                                    "end_date": "2019-07-20",
                                    "premium": 994
                                }
                            ],
                            "product_type": "camera",
                            "model": "camera_nikon_9",
                            "serial_number": "1234567890",
                            "purchase_date": "2019-06-27",
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
                        }
                    ],
                    "policyholder_age": 24
                },
                "created_at": "2019-06-27T12:24:51.209Z",
                "currency": "ZAR"
            }
        ]
    }
}
```
<aside class="warning">
This request requires the device authentication in the header.
</aside>

### On Demand Quote - Has a Policy (user has previously activated an item)

### HTTP Request

`POST https://app.capsured.co.za/api/quote`

> EXAMPLE REQUEST

```shell
curl https://app.capsured.co.za/api/quote \
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
curl https://app.capsured.co.za/api/quote \
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

## Activate an Item

Activate an item for the authenticated user. This endpoint will activate the item on the policy of
the user with the quote he/she has chosen. This endpoint works in two ways:

1. When the user has no items activated, the user has no existing policy. The quote that the user got will contain a `package_id`. Thus, the `quote_package_id` will need
to be specified in the body of this request.
2. When the user has items activated and a policy already exists, the `quote_request` in the body needs
to be specified with the information returned from the `api/quote` request. 

The `card_id` can be specified on the request. This will only be added on requests following the first activation, since the card
will only be linked to the user after the first item has been activated.

### HTTP Request - First Item for the User

`POST https://app.capsured.co.za/api/item/activate`

> EXAMPLE REQUEST

```shell
curl https://app.capsured.co.za/api/item/activate \
  --request POST \
  -H "Content-Type: application/json" \
  -H "Authorization: Basic device_id:access_token" \
  -F 'display_0=@/path/to/file/example_file_1.jpg' \
  -F 'display_1=@/path/to/file/example_file_2.jpg' \
  -F 'display_2=@/path/to/file/example_file_3.jpg' \
  -F 'serial_0=@/path/to/file/example_file_4.jpg' \
  -b '{
    "id": 1,
    "cover_type": "on_demand",
    "quote_package_id": "cb0b50e1-128f-4146-8804-17d1bcafa6cb",
    "quote_request": {
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
                    "start_date": "2019-04-05",
                    "end_date": "2019-04-20"
                  }
                ]
            }
        ]	
    }
   }'
```

### HTTP Request - Second or more Item for the User

`POST https://app.capsured.co.za/api/item/activate`

> EXAMPLE REQUEST

```shell
curl https://app.capsured.co.za/api/item/activate \
  --request POST \
  -H "Content-Type: application/json" \
  -H "Authorization: Basic device_id:access_token" \
  -F 'display_0=@/path/to/file/example_file_1.jpg' \
  -F 'display_1=@/path/to/file/example_file_2.jpg' \
  -F 'display_2=@/path/to/file/example_file_3.jpg' \
  -F 'serial_0=@/path/to/file/example_file_4.jpg' \
  -b '{
    "id": 1,
    "cover_type": "on_demand",
    "quote_request": {
        "covered_items": [
            {
                "product_type": "camera",
                "cover_type": "on_demand",
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
                "cover_periods": [
                  {
                    "start_date": "2019-07-05",
                    "end_date": "2019-07-20",
                  }
                ]
            }
        ]	
    }
   }'
```

#### BODY PARAMETERS

Parameter | Description      
--------- |  -----------
id |  *integer*. The item's id.
cover_type      |  *string*. The cover type of the quote - `on_demand` or `monthly`.
quote_request <sub>optional</sub>   |  *json*. The quote that was given as a response from the `/api/quote` request.
quote_package_id <sub>optional</sub>   |  *string*. The quote package id that was selected for the firt activated item.
card_id <sub>optional</sub>   |  *string*. The card id of the user. This is only needed if a card is already active for the user.
serial_0 |  *file*. The serial number image of the item.
display_0 |  *file*. An image of the item
display_1 |  *file*. An image of the item
display_2 |  *file*. An image of the item
> EXAMPLE RESPONSE

```
 // TODO
```
<aside class="warning">
This request requires the device authentication in the header.
</aside>

## Deactivate an Item

Deactivate an item for the authenticated user. If an item is currently on an active policy, the
item will be removed from the policy and deactivated.

### HTTP Request

`PUT https://app.capsured.co.za/api/item/deactivate`

> EXAMPLE REQUEST

```shell
curl https://app.capsured.co.za/api/item/deactivate \
  --request PUT \
  -H "Content-Type: application/json" \
  -H "Authorization: Basic device_id:access_token" \
  -b '{
    "id": 1,
    "reason": "Because I can beep beep like a sheep."
  }'
```

#### BODY PARAMETERS

Parameter | Description      
--------- |  -----------
id |  *integer*. The item's id.
reason |  *string*. The reason for the cancellation.

> EXAMPLE RESPONSE

```
 // TODO
```
<aside class="warning">
This request requires the device authentication in the header.
</aside>

## Claim an Item

Claim an item for the authenticated user. This will issue a claim for the item selected and
send an email to the user and capsured to inform them a claim has been submitted. The process
after that is manual and this application has nothing to do with the claiming process further.

### HTTP Request

`PUT https://app.capsured.co.za/api/item/claim`

> EXAMPLE REQUEST

```shell
curl https://app.capsured.co.za/api/item/claim \
  --request PUT \
  -H "Content-Type: application/json" \
  -H "Authorization: Basic device_id:access_token" \
  -b '{
    "id": 1,
    "reason": "theft",
    "pssa_member": true,
    "incident_date": "2019-06-01"
  }'
```

#### BODY PARAMETERS

Parameter | Description      
--------- |  -----------
id |  *integer*. The item's id.
reason |  *string*. The reason for the claim.
pssa_member |  *boolean*. If the user is a `pssa member` or not.
incident_date |  *string*. The date of the incident.

> EXAMPLE RESPONSE

```
 // TODO
```
<aside class="warning">
This request requires the device authentication in the header.
</aside>

## Get Items

Get all the items of the authenticated user. If an item has a policy active, the `policy` tag in
the response wil contain all the information about the specific policy.

### HTTP Request

`GET https://app.capsured.co.za/api/items`

> EXAMPLE REQUEST

```shell
curl https://app.capsured.co.za/api/items \
  --request GET \
  -H "Content-Type: application/json" \
  -H "Authorization: Basic device_id:access_token"
```

> EXAMPLE RESPONSE

```json
 {
     "success": true,
     "errorCode": 0,
     "msg": "",
     "info": "",
     "data": {
         "items": [
             {
                 "id": 1,
                 "name": "Canon EOS M6 Compact Mirrorless Camera (Body Only, Black)",
                 "key": "camera_canon_10",
                 "product_type": "camera",
                 "make": "Canon",
                 "model": "Canon EOS M6 Compact Mirrorless Camera (Body Only, Black)",
                 "serial_number": "Hdjsjjdnnd",
                 "covered_item_id": null,
                 "reason_cancelled": "Test",
                 "status": 1,
                 "cover_type": 2,
                 "created_at": "2019-04-12 08:20:59",
                 "updated_at": "2019-04-12 08:23:57",
                 "deleted_at": null,
                 "purchase_date": "2019-04-10 00:00:00",
                 "policy": null,
                 "covered_items": null,
                 "files": [
                     {
                         "id": 1,
                         "path": "production/users/1/items/5/U4B1mS4WDkR27EFbBIuj1dMe7C0VvFW2zdNpg6Ja.jpeg",
                         "file_name": "profile_img_0_1555057257778_201",
                         "type": "profile"
                     },
                     {
                         "id": 2,
                         "path": "production/users/1/items/5/l8t5d6MaRumJKb390ws3JdS5CxrZ66G6amIxj7fE.jpeg",
                         "file_name": "display_0_img_1555057327385_633",
                         "type": "display"
                     },
                     {
                         "id": 3,
                         "path": "production/users/1/items/5/C4upO0XTgFa4g5YI075fGv43FYuvGD5UAG1rlIPN.jpeg",
                         "file_name": "display_1_img_1555057327385_215",
                         "type": "display"
                     },
                     {
                         "id": 4,
                         "path": "production/users/1/items/5/CdFfhOfMU7bkGfuqicpEs9Jbuju73jcdCT0kQ9VG.jpeg",
                         "file_name": "display_2_img_1555057327385_169",
                         "type": "display"
                     },
                     {
                         "id": 5,
                         "path": "production/users/1/items/5/WeugtqkUwUL1TyJgbcoBqGHjRewIr51I1oEfk0Zd.jpeg",
                         "file_name": "serial_1_img__1555057327385_759",
                         "type": "serial"
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

## Remove an Item

Remove an item from the authenticated user. An item can only be delete if it is not 
currently on an active policy.

### HTTP Request

`DELETE https://app.capsured.co.za/api/items/<id>/delete`

> EXAMPLE REQUEST

```shell
curl https://app.capsured.co.za/api/item/<id>/delete \
  --request DELETE \
  -H "Content-Type: application/json" \
  -H "Authorization: Basic device_id:access_token"
```

#### URL PARAMETERS

Parameter | Description      
--------- |  -----------
id |  *integer*. The item's id.

> EXAMPLE RESPONSE

```json
{
    "success": true,
    "errorCode": 0,
    "msg": "",
    "info": "",
    "data": {
        "item": {
            "id": 1,
            "name": "Canon EOS 4000D DSLR Camera (Body Only)",
            "key": "camera_canon_1",
            "product_type": "camera",
            "make": "Canon",
            "model": "Canon EOS 4000D DSLR Camera (Body Only)",
            "serial_number": "Test",
            "covered_item_id": null,
            "reason_cancelled": "I don't want to be billed",
            "status": 1,
            "cover_type": 2,
            "created_at": "2019-04-11 21:18:53",
            "updated_at": "2019-06-18 13:13:28",
            "deleted_at": {
                "date": "2019-06-18 13:13:28.413784",
                "timezone_type": 3,
                "timezone": "UTC"
            },
            "payment_successful": 0,
            "purchase_date": "2019-04-02 00:00:00",
            "files": [
                {
                    "id": 1,
                    "path": "production/users/1/items/3/GCWzKsAskboqcX8XTo0ttJzPCf5EgXpOvF4MKLiv.jpeg",
                    "file_name": "profile_img_0_1555017531540_186",
                    "type": "profile"
                },
                {
                    "id": 2,
                    "path": "production/users/1/items/3/311KysSrANlP5FnUuBJ55pmjWpo0N2bn0Vksmv3w.jpeg",
                    "file_name": "display_0_img_1555056918116_750",
                    "type": "display"
                },
                {
                    "id": 3,
                    "path": "production/users/1/items/3/vC9RMPGPJw8XIyBpZYXJCwnycMbLXM6OrFx1wYh8.jpeg",
                    "file_name": "display_1_img_1555056918116_168",
                    "type": "display"
                },
                {
                    "id": 4,
                    "path": "production/users/1/items/3/TnLnqzzWnsp5V2IZdJuxRySgVtgQYN9wwi9ddrpo.jpeg",
                    "file_name": "display_2_img_1555056918116_672",
                    "type": "display"
                },
                {
                    "id": 5,
                    "path": "production/users/1/items/3/NoP3s85nDjjAMGDpzig0iHiEPNQr7HFxu40fqZeM.jpeg",
                    "file_name": "serial_1_img__1555056918116_159",
                    "type": "serial"
                }
            ]
        }
    }
}
```
<aside class="warning">
This request requires the device authentication in the header.
</aside>

# Payments

## Get Card Page

To add a card for the user, the card page needs to be loaded from ROOT. The following endpoint
loads this view.

For testing purposes, you can use the following card details:

`4242 4242 4242 4242` (does not ask 3D secure).<br>
`4111 1111 1111 1111` (asks 3D secure, then you can simulate a failed payment as well).

The rest of the information for the card details can be anything you want, but still in a valid format.

<aside class="warning">
Once the card was successfully added, the first item will be activated on the policy. A card is then linked
to the user. Any following activation of items won't need to add a new card, but simply specify the current
card's id in the <code>/api/item/activate</code> request. Thus, adding a card only happens once.
</aside>


### HTTP Request

`GET https://app.capsured.co.za/api/card/<device_id>/<item_id>`

> EXAMPLE REQUEST

```shell
curl https://app.capsured.co.za/api/card/05B805D4-1454-4AE3-9F94-C290AB708137/1 \
  --request GET \
  -H "Authorization: Basic device_id:access_token"
```

#### URL PARAMETERS

Parameter | Description      
--------- |  -----------
device_id |  *string*. The device id of the user.
item_id   | *integer*. The item's id.

> EXAMPLE RESPONSE

```
    // This should be opened in the browser 
    // and will load a view where a card can be added
```

## Get Completed Payment Page

One a payment has been completed from root, they will redirect to a pre-specified link that we can provide.
This is the link that root will redirect to after a payment has failed or succeeded.

### HTTP Request

`GET https://app.capsured.co.za/api/payment/completed/<id>?successful=<successful>`

> EXAMPLE REQUEST

```shell
curl https://app.capsured.co.za/api/payment/completed/1?successful=true \
  --request GET \
  -H "Authorization: Basic device_id:access_token"
```

#### URL PARAMETERS

Parameter | Description      
--------- |  -----------
id |  *string*. The item's policy encrypted id (for security purposes).
successful   | *boolean*. If the payment was successful or not - `true` or `false`.

> EXAMPLE RESPONSE

```
    // This should be opened in the browser 
    // and will load a view where a payment was successful or not.
    // Currently a button is displayed to redirect back to the app.
```

# ROOT

## Get Devices

Gets the devices information straight from the root API.

### HTTP Request

`GET https://app.capsured.co.za/api/devices`

> EXAMPLE REQUEST

```shell
curl https://app.capsured.co.za/api/devices \
  --request GET \
  -H "Authorization: Basic device_id:access_token"
```

> EXAMPLE RESPONSE

```json
{
    "success": true,
    "errorCode": 0,
    "msg": "",
    "info": "",
    "data": {
        "devices": [
            {
                "key": "camera_canon_0",
                "id": "0",
                "product_type": "camera",
                "make": "Canon",
                "model": "Other",
                "price": "",
                "type": ""
            },
            {
                "key": "camera_canon_1",
                "id": "1",
                "product_type": "camera",
                "make": "Canon",
                "model": "Canon EOS 4000D DSLR Camera (Body Only)",
                "price": "3 275",
                "type": "DSLR Cameras"
            },
            {
                "key": "camera_canon_10",
                "id": "10",
                "product_type": "camera",
                "make": "Canon",
                "model": "Canon EOS M6 Compact Mirrorless Camera (Body Only, Black)",
                "price": "7 895",
                "type": "Mirrorless Cameras"
            }
            
            // ...
        ]
    }
}
            
```

<aside class="warning">
This request requires the device authentication in the header.
</aside>

## Get Makes

Gets the makes information straight from the root API.

### HTTP Request

`GET https://app.capsured.co.za/api/makes`

> EXAMPLE REQUEST

```shell
curl https://app.capsured.co.za/api/makes \
  --request GET \
  -H "Authorization: Basic device_id:access_token"
```

> EXAMPLE RESPONSE

```json
{
    "success": true,
    "errorCode": 0,
    "msg": "",
    "info": "",
    "data": {
        "makes": [
            {
                "key": "canon",
                "label": "Canon"
            },
            {
                "key": "fujifilm",
                "label": "Fujifilm"
            },
            {
                "key": "irix",
                "label": "Irix"
            },
            {
                "key": "kenko",
                "label": "Kenko"
            },
            {
                "key": "leica",
                "label": "Leica"
            },
            {
                "key": "nikon",
                "label": "Nikon"
            },
            {
                "key": "olympus",
                "label": "Olympus"
            },
            {
                "key": "panasonic",
                "label": "Panasonic"
            },
            {
                "key": "sigma",
                "label": "Sigma"
            },
            {
                "key": "sony",
                "label": "Sony"
            },
            {
                "key": "tamron",
                "label": "Tamron"
            },
            {
                "key": "tokina",
                "label": "Tokina"
            },
            {
                "key": "zeiss",
                "label": "ZEISS"
            }
        ]
    }
}
            
```

<aside class="warning">
This request requires the device authentication in the header.
</aside>

## Get Meta Data

Gets the meta data information straight from the root API.

### HTTP Request

`GET https://app.capsured.co.za/api/meta`

> EXAMPLE REQUEST

```shell
curl https://app.capsured.co.za/api/meta \
  --request GET \
  -H "Authorization: Basic device_id:access_token"
```

> EXAMPLE RESPONSE

```json
{
    "success": true,
    "errorCode": 0,
    "msg": "",
    "info": "",
    "data": {
        "meta_data": {
            "product_types": [
                {
                    "key": "camera",
                    "label": "Camera"
                },
                {
                    "key": "camera_lens",
                    "label": "Camera lens"
                }
            ],
            "usage": [
                {
                    "key": "business",
                    "label": "Business"
                },
                {
                    "key": "business_and_personal",
                    "label": "Business and Personal"
                },
                {
                    "key": "personal",
                    "label": "Personal"
                },
                {
                    "key": "personal_gym_and_fitness",
                    "label": "Personal: Predominantly Gym and Fitness"
                },
                {
                    "key": "personal_other",
                    "label": "Personal: Other"
                },
                {
                    "key": "personal_hobby",
                    "value": "Personal: Hobby"
                },
                {
                    "key": "peronal_racing",
                    "value": "Personal: Racing"
                },
                {
                    "key": "business_general_remote_sensing",
                    "value": "Business: General Remote Sensing"
                },
                {
                    "key": "business_agriculture_farming",
                    "value": "Business: Agriculture or Farming"
                },
                {
                    "key": "business_wildlife_monitoring",
                    "value": "Business: Wildlife monitoring"
                },
                {
                    "key": "business_commercial_aerial_surveillance_and_security",
                    "value": "Business: Commerical Aerial Surveillance and Security"
                },
                {
                    "key": "business_professional_photography_and_videography",
                    "value": "Business: Professional Photography and Videography"
                },
                {
                    "key": "business_oil_gas_and_mineral_exploration",
                    "value": "Business: Oil, Gas and Mineral Exploration"
                },
                {
                    "key": "business_real_estate_and_construction",
                    "value": "Business: Real Estate and Construction"
                },
                {
                    "key": "business_disaster_management",
                    "value": "Business: Disaster Management"
                },
                {
                    "key": "business_search_rescue_and_healthcare",
                    "value": "Business: Search, Rescue and Healthcare"
                },
                {
                    "key": "business_geographic_mapping",
                    "value": "Business: Geographic Mapping"
                },
                {
                    "key": "business_storm_tracking_forecasting",
                    "value": "Business: Storm Tracking and Forecasting"
                },
                {
                    "key": "business_deliveries",
                    "value": "Business: Deliveries"
                },
                {
                    "key": "business_other",
                    "value": "Business: Other"
                }
            ],
            "storage": [
                {
                    "key": "not_applicable",
                    "label": "Not applicable"
                },
                {
                    "key": "locked_safe",
                    "label": "Locked safe"
                },
                {
                    "key": "inside_house",
                    "label": "Inside house"
                },
                {
                    "key": "with_policyholder",
                    "label": "With policyholder"
                },
                {
                    "key": "at_workplace",
                    "label": "At workplace"
                },
                {
                    "key": "inside_car",
                    "label": "Inside car"
                }
            ],
            "benefits": [
                {
                    "key": "standard",
                    "label": "Standard"
                },
                {
                    "key": "executive",
                    "label": "Executive"
                }
            ],
            "equipment": [
                {
                    "key": "camera_bag",
                    "label": "Camera bag"
                },
                {
                    "key": "tripod",
                    "label": "Tripod"
                },
                {
                    "key": "memory_card",
                    "label": "Memory card"
                },
                {
                    "key": "hard_drive",
                    "label": "Hard-drive"
                },
                {
                    "key": "remote_control",
                    "label": "Remote control"
                },
                {
                    "key": "reflectors",
                    "label": "Reflectors"
                },
                {
                    "key": "lighting_equipment",
                    "label": "Lighting equipment"
                }
            ],
            "product_usage": {
                "camera": [
                    "business",
                    "personal",
                    "business_and_personal"
                ],
                "camera_lens": [
                    "business",
                    "personal",
                    "business_and_personal"
                ]
            },
            "product_storage": {
                "camera": [
                    "not_applicable",
                    "locked_safe",
                    "inside_house",
                    "inside_car",
                    "at_workplace"
                ],
                "camera_lens": [
                    "not_applicable",
                    "locked_safe",
                    "inside_house",
                    "inside_car",
                    "at_workplace"
                ]
            },
            "product_benefits": {
                "camera": [
                    "standard",
                    "executive"
                ],
                "camera_lens": [
                    "standard",
                    "executive"
                ]
            },
            "product_equipment": {
                "camera": [
                    {
                        "key": "camera_bag",
                        "max_sum_assured": 200000
                    },
                    {
                        "key": "tripod",
                        "max_sum_assured": 200000
                    },
                    {
                        "key": "memory_card",
                        "max_sum_assured": 200000
                    },
                    {
                        "key": "hard_drive",
                        "max_sum_assured": 200000
                    },
                    {
                        "key": "remote_control",
                        "max_sum_assured": 200000
                    },
                    {
                        "key": "reflectors",
                        "max_sum_assured": 200000
                    },
                    {
                        "key": "other",
                        "max_sum_assured": 200000
                    }
                ],
                "camera_lens": [
                    {
                        "key": "camera_bag",
                        "max_sum_assured": 200000
                    },
                    {
                        "key": "tripod",
                        "max_sum_assured": 200000
                    },
                    {
                        "key": "memory_card",
                        "max_sum_assured": 200000
                    },
                    {
                        "key": "hard_drive",
                        "max_sum_assured": 200000
                    },
                    {
                        "key": "remote_control",
                        "max_sum_assured": 200000
                    },
                    {
                        "key": "reflectors",
                        "max_sum_assured": 200000
                    },
                    {
                        "key": "other",
                        "max_sum_assured": 200000
                    }
                ]
            },
            "excess": {
                "fixed": [
                    {
                        "value": 50000
                    },
                    {
                        "value": 150000
                    }
                ],
                "percentage": [
                    {
                        "value": 5,
                        "min_excess": 100000
                    },
                    {
                        "value": 10,
                        "min_excess": 100000
                    }
                ]
            }
        }
    }
}
            
```

<aside class="warning">
This request requires the device authentication in the header.
</aside>

## Get Product Types

Gets the product types information of the root module straight from the root API.

### HTTP Request

`GET https://app.capsured.co.za/api/product_types`

> EXAMPLE REQUEST

```shell
curl https://app.capsured.co.za/api/product_types \
  --request GET \
  -H "Authorization: Basic device_id:access_token"
```

```json
{
    "success": true,
    "errorCode": 0,
    "msg": "",
    "info": "",
    "data": {
        "product_types": [
            {
                "key": "camera",
                "label": "Camera"
            },
            {
                "key": "camera_lens",
                "label": "Camera lens"
            }
            
            // ...
        ]
    }
}            
```

<aside class="warning">
This request requires the device authentication in the header.
</aside>

## Get Filtered Devices

Gets the devices information straight from the root API and filters them according to
the specified parameters.

### HTTP Request

`POST https://app.capsured.co.za/api/filtered_devices`

> EXAMPLE REQUEST

```shell
curl https://app.capsured.co.za/api/filtered_devices \
  --request POST \
  -H "Content-Type: application/json" \
  -H "Authorization: Basic device_id:access_token" \
  -b '{
      "product_type": "camera",
      "make": "Canon"
    }'
```

#### BODY PARAMETERS

Parameter | Description      
--------- |  -----------
product_type |  *string*. The type of the product - `camera_lens` or `camera`.
make | *string*. The make of the product (selected from the `api/makes` call) - `Canon`, `Nikon`, etc.

> EXAMPLE RESPONSE

```json
{
    "success": true,
    "errorCode": 0,
    "msg": "",
    "info": "",
    "data": {
        "devices": [
            {
                "key": "camera_canon_0",
                "id": "0",
                "product_type": "camera",
                "make": "Canon",
                "model": "Other",
                "price": "",
                "type": ""
            },
            {
                "key": "camera_canon_1",
                "id": "1",
                "product_type": "camera",
                "make": "Canon",
                "model": "Canon EOS 4000D DSLR Camera (Body Only)",
                "price": "3 275",
                "type": "DSLR Cameras"
            },
            {
                "key": "camera_canon_10",
                "id": "10",
                "product_type": "camera",
                "make": "Canon",
                "model": "Canon EOS M6 Compact Mirrorless Camera (Body Only, Black)",
                "price": "7 895",
                "type": "Mirrorless Cameras"
            }
            
            // ...
        ]
    }
}        
```

<aside class="warning">
This request requires the device authentication in the header.
</aside>

# General

## Get About

Get the about information.

### HTTP Request

`GET https://app.capsured.co.za/api/about`

> EXAMPLE REQUEST

```shell
curl https://app.capsured.co.za/api/about \
  --request GET
```

> EXAMPLE RESPONSE

```json
    {
        "success": true,
        "errorCode": 0,
        "msg": "",
        "info": "",
        "data": {
            "about": "Never gonna give you up ... "
        }
    }
```

## Get Terms and Conditions

Get the terms and conditions information.

### HTTP Request

`GET https://app.capsured.co.za/api/terms`

> EXAMPLE REQUEST

```shell
curl https://app.capsured.co.za/api/terms \
  --request GET
```

> EXAMPLE RESPONSE

```json
    {
        "success": true,
        "errorCode": 0,
        "msg": "",
        "info": "",
        "data": {
            "terms": "Never gonna let you down ... "
        }
    }
```