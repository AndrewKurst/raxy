# Raxy api design

##### Endpoints:

    POST: /resource/action/x/y
    POST: /resource/action/x
    POST: /resource/action/_/y
    POST: /resource/action

##### Request:

    {
      "meta": {
        "limit": 0,
        "offset": 0,
        "sortBy": "",
        "orderBy": ""
      },
      "data": [
        {
          "key": "",
          "value": {}
        }
      ]
    }

##### Status code:

`200` for all occasions. Check `meta.status` and `meta.errors` for more information.

##### Response:

    {  
      "meta": {  
        "status": "success",  
        "total": 0,  
        "errors": [  
          {  
            "key": "",
            "property": "",
            "value": ""
          }  
        ]  
      },  
      "data": [  
        {  
          "key": "",
          "value": {}  
        }  
      ]  
    }

### Glossary:

`resource` is a name of a resource. If `resource` is a collection, each element must have a unique identifier.

`action` is an action applied to `resource`.

`x` is a name for `request`. Allows overloading `action` depending on `request`.

`y` is a name for `response`. Allows overloading `action` depending on `response`.

`_` is a reduction for default `request` name.

`request` is a request body. Consists of `meta` and `data`.

`response` is a response body. Consists of `meta` and `data`.

`meta` is an additional data for `request` or `response`.

`data` is a payload of `request` or `response`. It is always a collection. Each element of `data` consists of `key` and `value`.

`key` is a temporary key generated on a client or on a server side. Allows linking `request` `data` element to `response` `data` element.

`value` is a model.

### Examples:

#### Add users:

    POST: /users/add 

x:

    {
      "meta": {},
      "data": [
        {
          "key": "b1350027-f703-47c7-8571-09611246ab3c",
          "value": {
            "email": "andrewkurst@gmail.com",
            "name": "Andrew"
          }
        },
        {
          "key": "ae7d54a2-8af3-4c21-b4b9-9cd4f7f82015",
          "value": {
            "email": "ivanivanov@gmail.com",
            "name": "Ivan Ivanov"
          }
        }
      ]
    }

y:

    {  
      "meta": {  
        "status": "success",  
        "total": 2,  
        "errors": []  
      },  
      "data": [  
        {  
          "key": "b1350027-f703-47c7-8571-09611246ab3c",
          "value": {  
            "id": "4ba5c2ab-0f82-4bc8-bede-9d933345b5f3",  
            "email": "andrewkurst@gmail.com",  
            "name": "Andrew"  
          }  
        },  
        {  
          "key": "ae7d54a2-8af3-4c21-b4b9-9cd4f7f82015",
          "value": {  
            "id": "f8ad8018-5eba-450d-96af-965a42f4dbfb",  
            "email": "ivanivanov@gmail.com",  
            "name": "Ivan Ivanov"  
          }  
        }  
      ]  
    }

#### Add admins:

    POST: /users/add/admins

x:

    {
      "meta": {},
      "data": [
        {
          "key": "79a57028-cefe-499f-9e85-2dc15a07f06d",
          "value": {
            "email": "superadmin@gmail.com",
            "name": "Super Admin",
            "role": "super_admin"
          }
        }
      ]
    }

y:

    {
      "meta": {
        "status": "success",
        "total": 1,
        "errors": []
      },
      "data": [
        {
          "key": "79a57028-cefe-499f-9e85-2dc15a07f06d",
          "value": {
            "id": "a32c502a-b221-438f-bef6-7fd2a9b2105a",
            "email": "superadmin@gmail.com",
            "name": "Super Admin",
            "role": "super_admin"
          }
        }
      ]
    }

#### Delete users by ids:

    POST: /users/delete/by_ids

x:

    {
      "meta": {},
      "data": [
        {
          "key": "98d6b811-4794-432c-96c9-62c3c74c1338",
          "value": "f8ad8018-5eba-450d-96af-965a42f4dbfb"
        }
      ]
    }

y:

    {
      "meta": {
        "status": "success",
        "total": 0,
        "errors": []
      },
      "data": [
        {
          "key": "98d6b811-4794-432c-96c9-62c3c74c1338",
          "value": {}
        }
      ]
    }  

#### Edit users:

    POST: /users/edit

x:

    {
      "meta": {},
      "data": [
        {
          "key": "aaf32dce-94f0-4b2c-b67f-a3eb15397056",
          "value": {
            "id": "4ba5c2ab-0f82-4bc8-bede-9d933345b5f3",
            "email": "andrewkurst@gmail.com",
            "name": "Andrew Kurst"
          }
        }
      ]
    }

y:

    {
      "meta": {
        "status": "success",
        "total": 1,
        "errors": []
      },
      "data": [
        {
          "key": "aaf32dce-94f0-4b2c-b67f-a3eb15397056",
          "value": {
            "id": "4ba5c2ab-0f82-4bc8-bede-9d933345b5f3",
            "email": "andrewkurst@gmail.com",
            "name": "Andrew Kurst"
          }
        }
      ]
    }

#### Get users:

    POST: /users/get

x:

    {
      "meta": {},
      "data": []
    }

y:

    {
      "meta": {
        "status": "success",
        "total": 2,
        "errors": []
      },
      "data": [
        {
          "key": "464b586f-1d87-44a1-8646-7a7188e71323",
          "value": {
            "id": "4ba5c2ab-0f82-4bc8-bede-9d933345b5f3",
            "email": "andrewkurst@gmail.com",
            "name": "Andrew Kurst"
          }
        },
        {
          "key": "4f305a3f-0a39-49dd-b71d-cff3e60d81f6",
          "value": {
            "id": "a32c502a-b221-438f-bef6-7fd2a9b2105a",
            "email": "superadmin@gmail.com",
            "name": "Super Admin"
          }
        }
      ]
    }

#### Get admins:

    POST: /users/get/admins

x:

    {
      "meta": {},
      "data": []
    }

y:

    {
      "meta": {
        "status": "success",
        "total": 1,
        "errors": []
      },
      "data": [
        {
          "key": "a4d8f686-518a-4152-9205-a2a776cf417a",
          "value": {
            "id": "a32c502a-b221-438f-bef6-7fd2a9b2105a",
            "email": "superadmin@gmail.com",
            "name": "Super Admin"
          }
        }
      ]
    }

#### Get users' emails:

    POST: /users/get/_/to_emails

x:

    {
      "meta": {},
      "data": []
    }

y:

    {
      "meta": {
        "status": "success",
        "total": 2,
        "errors": []
      },
      "data": [
        {
          "key": "fe70e756-1206-43e6-bd94-a78ead17e347",
          "value": "andrewkurst@gmail.com"
        },
        {
          "key": "bbffd152-49a6-4765-bfa1-02957f98675e",
          "value": "superadmin@gmail.com"
        }
      ]
    }

#### Get me:

    POST: /users/get/me

x:

    {
      "meta": {},
      "data": []
    }

y:

    {
      "meta": {
        "status": "success",
        "total": 0,
        "errors": []
      },
      "data": []
    }

#### Get users by filter:

    POST: /users/get/by_filters

x:

    {
      "meta": {},
      "data": [
        {
          "key": "64012687-2380-4287-bac4-36ed3148d5ad",
          "value": {
              "property": "email",
              "value": "andrewkurst@gmail.com",
              "operator": "=",
              "group": 1
            }
        }
      ]
    }

y:

    {
      "meta": {
        "status": "success",
        "total": 1,
        "errors": []
      },
      "data": [
        {
          "key": "7e514af6-51fb-48c1-9c6b-c2b53b2bfb79",
          "value": {
            "id": "4ba5c2ab-0f82-4bc8-bede-9d933345b5f3",
            "email": "andrewkurst@gmail.com",
            "name": "Andrew Kurst"
          }
        }
      ]
    }

#### Get users' ids by filter

    POST: /users/get/by_filters/to_ids

x:

    {
      "meta": {},
      "data": [
        {
          "key": "f6633c19-6c4b-4c0d-919f-aec06c305435",
          "value": {
              "property": "email",
              "value": "andrewkurst@gmail.com",
              "operator": "=",
              "group": 1
            }
        }
      ]
    }

y:

    {
      "meta": {
        "status": "success",
        "total": 1,
        "errors": []
      },
      "data": [
        {
          "key": "d7b65742-bc3b-49c9-be7b-78796f28e0d4",
          "value": {
            "id": "4ba5c2ab-0f82-4bc8-bede-9d933345b5f3"
          }
        }
      ]
    }

#### Validate users' credentials

    POST: /users/validate/credentials

x:

    {
      "meta": {},
      "data": [
        {
          "key": "cf8578ec-cac9-4ee2-9ef2-8280e7c0c3dd",
          "value": {
            "login": "andrewkurst@gmail.com",
            "password": "pa$$w0rd"
          }
        },
        {
          "key": "47b8b875-c5d3-4861-a588-1b02afa8e132",
          "value": {
            "login": "superadmin@gmail.com",
            "password": "12345678"
          }
        }
      ]
    }  

y:

    {
      "meta": {
        "status": "success",
        "total": 1,
        "errors": []
      },
      "data": [
        {
          "key": "cf8578ec-cac9-4ee2-9ef2-8280e7c0c3dd",
          "value": true
        },
        {
          "key": "47b8b875-c5d3-4861-a588-1b02afa8e132",
          "value": false
        }
      ]
    }

#### Calculate sum:

    POST: /calculator/sum

x:

    {
      "meta": {},
      "data": [
        {
          "key": "3c083c87-809c-4afd-ba60-6df690056d92",
          "value": -5
        },
        {
          "key": "e4274356-ebce-4fc3-9d5c-742eb3d9563c",
          "value": 7
        } ,
        {
          "key": "1a61a03b-af7b-434a-adb7-ae6f723e968e",
          "value": 2.7
        }
      ]
    }

y:

    {
      "meta": {
        "status": "success",
        "total": 1,
        "errors": []
      },
      "data": [
        {
          "key": "b8ae9c4a-c125-474a-855e-0bc502d0f01d",
          "value": 4.7
        }
      ]
    }
