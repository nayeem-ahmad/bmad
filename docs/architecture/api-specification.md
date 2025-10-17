# API Specification

This section defines the contract for the REST API.

```yaml
openapi: 3.0.0
info:
  title: SaaS Platform for Grocery Shops API
  version: 1.0.0
  description: The REST API for the SaaS Platform for Grocery Shops, providing resources for managing retail operations.
servers:
  - url: /api
    description: API base path

# =====================================================================================
# Paths
# =====================================================================================
paths:
  /products:
    get:
      summary: List Products
      description: Retrieves a list of all products for the authenticated user's store.
      tags:
        - Products
      responses:
        '200':
          description: A JSON array of products.
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Product'
        '401':
          $ref: '#/components/responses/UnauthorizedError'

    post:
      summary: Create Product
      description: Creates a new product in the store's inventory.
      tags:
        - Products
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/NewProduct'
      responses:
        '201':
          description: The newly created product.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Product'
        '400':
          $ref: '#/components/responses/BadRequestError'
        '401':
          $ref: '#/components/responses/UnauthorizedError'

  /products/{productId}:
    get:
      summary: Get Product
      description: Retrieves a single product by its ID.
      tags:
        - Products
      parameters:
        - name: productId
          in: path
          required: true
          schema:
            type: string
            format: uuid
      responses:
        '200':
          description: A single product.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Product'
        '401':
          $ref: '#/components/responses/UnauthorizedError'
        '404':
          $ref: '#/components/responses/NotFoundError'

# =====================================================================================
# Components
# =====================================================================================
components:
  schemas:
    Product:
      type: object
      properties:
        id:
          type: string
          format: uuid
        store_id:
          type: string
          format: uuid
        name:
          type: string
        price:
          type: number
          format: decimal
        quantity:
          type: integer
        sku:
          type: string
          nullable: true

    NewProduct:
      type: object
      required:
        - name
        - price
        - quantity
      properties:
        name:
          type: string
        price:
          type: number
          format: decimal
        quantity:
          type: integer
        sku:
          type: string
          nullable: true

    ApiError:
      type: object
      properties:
        error:
          type: object
          properties:
            code:
              type: string
              example: "bad_request"
            message:
              type: string
              example: "Invalid input provided."
            timestamp:
              type: string
              format: date-time
            requestId:
              type: string
              format: uuid

  responses:
    UnauthorizedError:
      description: Authentication information is missing or invalid.
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/ApiError'
    BadRequestError:
      description: The request body is malformed or invalid.
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/ApiError'
    NotFoundError:
      description: The requested resource was not found.
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/ApiError'
```
