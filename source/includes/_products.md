# Products

## Get Products

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/products' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will return all products

```json
{
    "products": {
        "total": 2,
        "per_page": 10,
        "current_page": 1,
        "last_page": 1,
        "next_page_url": null,
        "prev_page_url": null,
        "from": 1,
        "to": 2,
        "data": [
            {
                "id": 2,
                "source_uid": null,
                "mailbox_id": null,
                "title": "Fluent Forms",
                "description": "",
                "settings": null,
                "source": "local",
                "created_by": null,
                "created_at": "2021-11-29 17:56:13",
                "updated_at": "2021-11-29 17:56:13"
            },
            {
                "id": 1,
                "source_uid": null,
                "mailbox_id": null,
                "title": "Fluent Support",
                "description": "",
                "settings": null,
                "source": "local",
                "created_by": null,
                "created_at": "2021-11-29 17:56:07",
                "updated_at": "2021-11-29 17:56:07"
            }
        ]
    }
}
```

This endpoint will return all products (paginated, ordered by title)

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/products`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
per_page | int | no | Records per page (default 15)
page | int | no | Page number for pagination (default 1)
search | string | no | Search products

## Get a Specific Product

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/products/<product_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will return a specific product by it's id

```json
{
    "product": {
        "id": 1,
        "source_uid": null,
        "mailbox_id": null,
        "title": "Fluent Support",
        "description": "",
        "settings": null,
        "source": "local",
        "created_by": null,
        "created_at": "2021-11-29 17:56:07",
        "updated_at": "2021-11-29 17:56:07"
    }
}
```

This endpoint will return a specific product by it's id

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/products/<product_id>`

## Create a New Product
```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/products' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--header 'Content-Type: application/json' \
--data-raw '{
    "title": "Fluent CRM",
    "description": ""
}'
```
> The above command will create a new product

```json
{
    "message": "Product has been successfully created",
    "product": {
        "title": "Fluent CRM",
        "description": "",
        "updated_at": "2021-12-09 16:11:13",
        "created_at": "2021-12-09 16:11:13",
        "id": 3
    }
}
```

This endpoint will create a new product

### HTTP Request
`POST https://yourdomain.com/wp-json/fluent-support/v2/products`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
title | string | yes | Product title
description | string | no | Product description

## Create a Product (Alternate Route)

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/products/<product_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--header 'Content-Type: application/json' \
--data-raw '{
    "title": "Fluent CRM",
    "description": ""
}'
```
> The above command will create a new product

```json
{
    "message": "Product has been successfully created",
    "product": {
        "title": "Fluent CRM",
        "description": "",
        "updated_at": "2021-12-09 16:11:13",
        "created_at": "2021-12-09 16:11:13",
        "id": 4
    }
}
```

This route is handled by the same create method as `POST /products`: the `<product_id>` in the URL is ignored and a **new** product is always created. It does not update the product. To update a product use [Update a Product](#update-a-product) (`PUT`).

### HTTP Request
`POST https://yourdomain.com/wp-json/fluent-support/v2/products/<product_id>`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
title | string | yes | Product title
description | string | no | Product description



## Update a Product

```shell
curl --location --request PUT 'https://yourdomain.com/wp-json/fluent-support/v2/products/<product_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--header 'Content-Type: application/json' \
--data-raw '{
    "title": "Fluent Support",
    "description": "This is for Fluent Support"
}'
```

> The above command will update a specific product by it's id

```json
{
    "message": "Product has been updated",
    "product": {
        "id": 1,
        "source_uid": "0",
        "mailbox_id": null,
        "title": "Fluent Support",
        "description": "This is for Fluent Support",
        "settings": "",
        "source": "local",
        "created_by": "0",
        "created_at": "2021-11-29 17:56:07",
        "updated_at": "2021-12-09 14:44:35"
    }
}
```

This endpoint will update a specific product by it's id

### HTTP Request
`PUT https://yourdomain.com/wp-json/fluent-support/v2/products/<product_id>`


### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
title | string | yes | Product title
description | string | no | Product description

## Delete a Product

```shell
curl --location --request DELETE 'https://yourdomain.com/wp-json/fluent-support/v2/products/<product_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will delete a specific product by it's id

```json
{
    "message": "Product has been deleted"
}
```

This endpoint will delete a specific product by it's id

### HTTP Request

`DELETE https://yourdomain.com/wp-json/fluent-support/v2/products/<product_id>`
