# PromoCodes
Promo codes API made with Symfony

----

## Stack
* [Symfony framework](https://symfony.com/doc/current/setup.html)
* [Docker](https://docs.docker.com/install/) (for database local testing)
* AWS Lambdas (TO DO)

## Description
* The project consist of multiple APIs made to be used as a backend for a promo codes system.
* Currently, the API allow you create, update, and validate promo codes.
* The API is authenticated using JWT tokens, being only tokens generated by our authenticated service.

## Routes details
* **/api/create/{owner}**:
    * **HTTP Method:** 
        * POST
    * **Query Parameters:** 
        * discount-percentage (optional: default is 10)
        * expiration-date (optional: default is moment of creation + 30 days)
        * description (optional)
        * revenue-share (optional: default is 0) 
    * **Description:** 
        * Creates a new promo code for the "owner". The "createdBy" is extracted from the authentication token.
    * **Return Values:**
        * Promo code id
       
* **/api/update/{promoCodeId}**:
    * **HTTP Method:** 
        * PATCH
    * **Query Parameters:** 
        * discount-percentage (optional)
        * expiration-date (optional)
        * description (optional)
        * revenue-share (optional)
    * **Description:** 
        * Update an existent promo code attributes.
    * **Return Values:**
        * Invalid promo code<br>Success message
       
* **/api/validate/{promoCodeId}/{userId}**:
    * **HTTP Method:** 
        * GET
    * **Query Parameters:** 
    * **Description:** 
        * Validate if a promo code is valid. The "userId" is the user that is trying to use this promo code (if any), this way we assure that the owner cannot use the own promo code.
    * **Return Values:**
        * Invalid promo code id
        * User cannot use own promo code
        * Promo code already expired
        * Valid promo code

## Run locally:
* Start and populate local database:
```bash
docker-compose up

bin/console doctrine:migration:diff (only needed if you changed some entity - it creates the new migrations version)
bin/console doctrine:migration:migrate

bin/console doctrine:fixtures:load
```

* Start the local server
```bash
symfony server:start (or php bin/console server:start)
```

* Then, use curl/postman to make requests
* __**Important**__: you will need to include a `'Authorization: BEARER ...'` header on your request. To get that token, you can login on ops and copy the token from cookies.

### Troubleshooting:
* If after running `docker-compose up` and trying to connect to DB/run migrations, you got errors, you have two possible solutions:
  * Close the running docker (ctrl+c), run `docker-compose stop` and then `docker-compose up` again;
  * If you still get errors, you can close the running docker, run `docker-compose down`, delete the `var/cache/` folder on your project, and finally run `docker-compose up`.

## Deploy:
* TODO with serverless

## TODO (promo code attributes):
* Tests and GitHub actions
* Deployment process
