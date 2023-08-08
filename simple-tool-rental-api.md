# Simple Tool Rental API #

This API allows you to reserve a tool. 

The API is available at `https://simple-tool-rental-api.glitch.me`

## Endpoints ##

- [Status](#Status)
- [Tools](#Tools)
  - [Get all tools](#Get-all-tools)
  - [Get a single tool](#Get-a-single-tool)
- [Orders](#Orders)
  - [Get all orders](#Get-all-orders)
  - [Get a single order](#Get-a-single-order)
  - [Create a new order](#Create-a-new-order)
  - [Update an order](#Update-an-order)
  - [Delete an order](#Delete-an-order)
- [API Authentication](#API-Authentication)
  - [Register a new API client](#Register-a-new-API-client)

## Status ##

**`GET /status`**

Returns the status of the API.

- `UP` indicates that the API is running as expected.
- `DOWN` indicates that the API is currently not working. 

## Tools ##

### Get all tools ###

**`GET /tools`**

Returns a list of tools from the inventory. 

**Parameters**

| Name       | Type    | In    | Description                                                                                                                                                     |
|------------|---------|-------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `category` | string  | query | Optional - Specifies the category of tools you want to be returned. It can be one of: ladders, plumbing, power-tools, trailers, electric-generators, lawn-care. |
| `results`  | integer | query | Optional - Specifies the number of results you want. Must be number between 1 and 20. By default, only 20 tools will be displayed.                              |                                                               |
| `available`| boolean | query | Optional - Specifies the availability of the tools. By default, all tools will be displayed.                                                                    |


**Status codes**
| Status code     | Description                                         |
|-----------------|-----------------------------------------------------|
| 200 OK          | Indicates a successful response.                    |
| 400 Bad Request | Indicates that the parameters provided are invalid. |

### Get a single tool ###

**`GET /tools/:toolId`**

Returns a single tool from the inventory. 

**Parameters**

| Name             | Type    | In   | Description                                        |
|------------------|---------|------|----------------------------------------------------|
| `toolId`         | integer | path | Specifies the id of the tool you wish to retrieve. |
| `user-manual`    | boolean | query| Optional - Returns the user manual in PDF format.  |


**Status codes**

| Status code     | Description                                            |
|-----------------|--------------------------------------------------------|
| 200 OK          | Indicates a successful response.                       |
| 404 Not found   | Indicates that there is no tool with the specified id. |

## Orders ##

### Get all orders ###

Returns all orders created by the API client. 

**`GET /orders`**

**Parameters**

| Name           | Type    | In     | Description                                   |
|----------------|---------|--------|-----------------------------------------------|
| `Authorization`| string  | header | Specifies the bearer token of the API client. |

**Status codes**

| Status code      | Description                                                                                         |
|------------------|-----------------------------------------------------------------------------------------------------|
| 200 OK           | Indicates a successful response.                                                                    |
| 401 Unauthorized | Indicates that request has not been authenticated. Check the response body for additional details.  |


### Get a single order ###

Returns a single order.

**`GET /orders/:orderId`**

**Parameters**

| Name            | Type    | In     | Description                                   |
|-----------------|---------|--------|-----------------------------------------------|
| `orderId`       | string  | path   | Specifies the order id.                       |
| `invoice`       | boolean | query  | Optional - Shows the PDF invoice              |
| `Authorization` | string  | header | Specifies the bearer token of the API client. |

**Status codes**

| Status code      | Description                                                                                         |
|------------------|-----------------------------------------------------------------------------------------------------|
| 200 OK           | Indicates a successful response.                                                                    |
| 401 Unauthorized | Indicates that request has not been authenticated. Check the response body for additional details.  |
| 404 Not found    | Indicates that there is no order with the specified id associated with the API client.              |


### Create a new order ###

**`POST /orders`**

**Parameters**

| Name            | Type    | In     | Description                                   |
|-----------------|---------|--------|-----------------------------------------------|
| `Authorization` | string  | header | Specifies the bearer token of the API client. |
| `toolId`        | integer | body   | Specifies the tool id.                        |
| `customerName`  | string  | body   | Specifies the name of the customer.           |
| `comment`       | string  | body   | Optional. Specifies a comment.                |

**Status codes**

| Status code      | Description                                                                                         |
|------------------|-----------------------------------------------------------------------------------------------------|
| 201 Created      | Indicates that the order has been created successfully.                                             |
| 400 Bad Request  | Indicates that the parameters provided are invalid.                                                 |
| 401 Unauthorized | Indicates that request has not been authenticated. Check the response body for additional details.  |

Example request body:

 ```
{
  "toolId": 1643,
  "customerName": "John Doe"
}
```

### Update an order ###

**`PATCH /orders/:orderId`**

**Parameters**

| Name            | Type    | In     | Description                                   |
|-----------------|---------|--------|-----------------------------------------------|
| `orderId`       | string  | path   | Specifies the order id.                       |
| `customerName`  | string  | body   | Optional. Specifies the name of the customer. |
| `comment`       | string  | body   | Optional. Specifies a comment.                |
| `Authorization` | string  | header | Specifies the bearer token of the API client. |


**Status codes**

| Status code      | Description                                                                                         |
|------------------|-----------------------------------------------------------------------------------------------------|
| 204 No Content   | Indicates that the order has been updated successfully.                                             |
| 400 Bad Request  | Indicates that the parameters provided are invalid.                                                 |
| 401 Unauthorized | Indicates that request has not been authenticated. Check the response body for additional details.  |
| 404 Not found    | Indicates that there is no order with the specified id associated with the API client.              |

Example request body:

 ```
{
  "customerName": "Joe Doe"
}
```

### Delete an order ###

**`DELETE /orders/:orderId`**

**Parameters**

| Name            | Type    | In     | Description                                   |
|-----------------|---------|--------|-----------------------------------------------|
| `orderId`       | string  | path   | Specifies the order id.                       |
| `Authorization` | string  | header | Specifies the bearer token of the API client. |


**Status codes**

| Status code      | Description                                                                                         |
|------------------|-----------------------------------------------------------------------------------------------------|
| 204 No Content   | Indicates that the order has been deleted successfully.                                             |
| 400 Bad Request  | Indicates that the parameters provided are invalid.                                                 |
| 401 Unauthorized | Indicates that request has not been authenticated. Check the response body for additional details.  |
| 404 Not found    | Indicates that there is no order with the specified id associated to the API client.                |


## API Authentication ##

Some endpoints may require authentication. To submit or view an order, you need to register your API client and obtain an access token.

The endpoints that require authentication expect a bearer token sent in the `Authorization` header. 

Example:

`Authorization: Bearer YOUR TOKEN`


### Register a new API client ###


**`POST /api-clients`**

The request body needs to be in JSON format.

| Name         | Type   | In   | Description                                    |
|--------------|--------|------|------------------------------------------------|
| `clientName` | string | body | Specifies the name of the API client.          |
| `clientEmail`| string | body | Specifies the email address of the API client. |

Note: You don't need to provide a real email address. 


**Status codes**
| Status code     | Description                                                                       |
|-----------------|-----------------------------------------------------------------------------------|
| 201 Created     | Indicates that the client has been registered successfully.                       |
| 400 Bad Request | Indicates that the parameters provided are invalid.                               |
| 409 Conflict    | Indicates that an API client has already been registered with this email address. |


Example request body:

 ```
 {
    "clientName": "Postman",
    "clientEmail": "valentin@example.com"
}
 ```
 
The response body will contain the access token.
