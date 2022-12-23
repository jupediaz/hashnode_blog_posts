# Creating Documentation and Testing with Swagger OpenAPI for a Laravel API

## **Introduction**

API documentation is an important part of the development process, as it helps to communicate the functionality of an API to developers who will be using it. [**Swagger OpenAPI**](https://swagger.io/) is a popular tool for generating API documentation, and it can be easily integrated into a Laravel API.

## **Setting up Swagger OpenAPI**

To set up **Swagger OpenAPI** for a Laravel API, we'll need to install the `swaggervel` package. This can be done by running the following command in the root directory of your Laravel project:

```bash
composer require "darkaonline/l5-swagger:5.7.*"
```

Once the package is installed, we need to add the service provider to the `providers` array in the `config/app.php` file:

```php
'providers' => [
    // Other service providers...
    Darkaonline\L5Swagger\L5SwaggerServiceProvider::class,
],
```

Next, we need to publish the configuration file and assets by running the following command:

```bash
php artisan vendor:publish --provider "Darkaonline\L5Swagger\L5SwaggerServiceProvider"
```

This will create a `config/l5-swagger.php` configuration file, as well as a `public/vendor/swagger-ui` directory containing the **Swagger UI** assets.

To define the structure and behavior of your API, you'll need to create a **Swagger OpenAPI** specification file in the `public/docs` directory. This file should be written in **YAML** or **JSON** and should define the API's endpoints, request and response bodies, and other details. Here's an example of a basic **Swagger OpenAPI** specification file for a Laravel API:

```yaml
openapi: 3.0.1
info:
  title: My Laravel API
  version: 1.0.0
servers:
  - url: https://api.example.com/
paths:
  /users:
    get:
      summary: Get a list of users
      operationId: getUsers
      tags:
        - users
      parameters:
        - name: limit
          in: query
          description: The maximum number of users to return
          required: false
          schema:
            type: integer
      responses:
        '200':
          description: A list of users
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/User'
components:
  schemas:
    User:
      type: object
      required:
        - id
        - name
      properties:
        id:
          type: integer
          description: The unique identifier for the user
        name:
          type: string
          description: The name of the user
```

In this example, the `openapi` and `info` blocks define the version of the **Swagger OpenAPI** specification and the API's metadata, respectively. The `servers` block defines the base URL for the API, and the `paths` block defines the API's endpoints and their behavior. The `components` block defines reusable schemas that can be referenced by the endpoints.

You can then access the documentation by visiting the `/docs` route in your Laravel application. The **Swagger UI** will display the documentation and allow users to interact with the API by making requests and viewing the responses.

## **Defining API Endpoints**

With **Swagger OpenAPI** set up, we can now start defining our API endpoints. We can do this by adding annotations to our controller methods. Here's an example of a simple API endpoint for creating a new user:

```php
/**
 * @OA\Post(
 *     path="/users",
 *     summary="Create a new user",
 *     @OA\RequestBody(
 *         required=true,
 *         @OA\JsonContent(
 *             @OA\Property(property="name", type="string"),
 *             @OA\Property(property="email", type="string"),
 *             @OA\Property(property="password", type="string")
 *         )
 *     ),
 *     @OA\Response(
 *         response=200,
 *         description="Successful operation"
 *     ),
 *     @OA\Response(
 *         response=400,
 *         description="Invalid input"
 *     ),
 *     @OA\Response(
 *         response=401,
 *         description="Unauthorized"
 *     ),
 *     @OA\Response(
 *         response=403,
 *         description="Forbidden"
 *     ),
 *     @OA\Response(
 *         response=404,
 *         description="Resource not found"
 *     )
 * )
 */
public function store(Request $request)
{
    // Validate and create the user...
}
```

In this example, we're using the `@OA\Post` annotation to define a POST request to the `/users` path, with a summary description and request body specification. We also define a set of possible responses, including a successful response (200), as well as responses for invalid input (400), unauthorized access (401), forbidden access (403), and a resource not found (404).

We can also specify additional details for each endpoint, such as query parameters and response examples. Here's an example of an endpoint that retrieves a list of users, with pagination and filtering options:

```php
/**
 * @OA\Get(
 *     path="/users",
 *     summary="Get a list of users",
 *     @OA\Parameter(
 *         name="page",
 *         in="query",
 *         description="The page number",
 *         required=false,
 *         @OA\Schema(type="integer")
 *     ),
 *     @OA\Parameter(
 *         name="per_page",
 *         in="query",
 *         description="The number of items per page",
 *         required=false,
 *         @OA\Schema(type="integer")
 *     ),
 *     @OA\Parameter(
 *         name="name",
 *         in="query",
 *         description="Filter by name",
 *         required=false,
 *         @OA\Schema(type="string")
 *     ),
 *     @OA\Response(
 *         response=200,
 *         description="Successful operation",
 *         @OA\JsonContent(
 *             type="array",
 *             @OA\Items(ref="#/components/schemas/User")
 *         )
 *     ),
 *     @OA\Response(
 *         response=401,
 *         description="Unauthorized"
 *     ),
 *     @OA\Response(
 *         response=403,
 *         description="Forbidden"
 *     ),
 *     @OA\Response(
 *         response=404,
 *         description="Resource not found"
 *     )
 * )
 */
public function index(Request $request)
{
    // Retrieve and return a list of users...
}
```

In this example, we use the `@OA\Get` annotation to define a GET request to the `/users` path, and define a set of query parameters using the `@OA\Parameter` annotation. We also specify a successful response (200) with a JSON array of users, using the `@OA\JsonContent` and `@OA\Items` annotations to define the array items and their schema.

## **Generating the Documentation**

With our API endpoints defined, we can now generate the documentation by running the following command:

```bash
php artisan l5-swagger:generate
```

This will generate a `public/docs/api-docs.json` file containing the **Swagger OpenAPI** specification for our API. We can then view the documentation by visiting the `/docs` route in our Laravel application. This will display the **Swagger UI**, which allows us to interactively explore the API and try out different requests.

## **Testing the API**

In addition to generating documentation, **Swagger OpenAPI** can also be used for testing our API. We can do this by using the **Swagger Codegen tool** to generate a client library for our API, which can then be used to make requests and assert on the responses.

To generate a client library using **Swagger Codegen**, we can run the following command:

```bash
java -jar swagger-codegen-cli.jar generate -i public/docs/api-docs.json -l <LANGUAGE> -o <OUTPUT_DIRECTORY>
```

Replace `<LANGUAGE>` with the desired language for the client library (e.g. `php`, `python`, `javascript`, etc.), and `<OUTPUT_DIRECTORY>` with the desired output directory. This will generate the client library in the specified output directory.

Once the client library is generated, we can use it to make requests to our API and assert on the responses. Here's an example of how to do this in PHP:

```php
$client = new Swagger\Client\Api\UsersApi();

$response = $client->createUser([
    'name' => 'John Doe',
    'email' => 'john@example.com',
    'password' => 'password123',
]);

$this->assertEquals(200, $response->getStatusCode());
$this->assertEquals('John Doe', $response->getData()->name);
```

In this example, we create a new instance of the `UsersApi` class from the generated client library, and use it to make a POST request to the `/users` endpoint. We then assert that the response has a status code of 200 and that the `name` field in the response data is correct.

We can also use the client library to test other API endpoints, such as retrieving a list of users:

```php
$client = new Swagger\Client\Api\UsersApi();

$response = $client->getUsers();

$this->assertEquals(200, $response->getStatusCode());
$this->assertCount(10, $response->getData());
```

In this example, we create a new instance of the `UsersApi` class and use it to make a GET request to the `/users` endpoint. We then assert that the response has a status code of 200 and that the `data` field in the response is an array with 10 items.

## **Conclusion**

In this post, we covered how to set up and use **Swagger OpenAPI** for generating documentation and testing a Laravel API.

We saw how to define API endpoints using annotations, generate the documentation, and use the **Swagger Codegen tool** to generate a client library for testing the API.

By following these steps, you can easily add documentation and testing to your Laravel API using **Swagger OpenAPI**.