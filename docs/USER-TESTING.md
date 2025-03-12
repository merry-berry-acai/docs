# User/Testing Documentation

## Route: GET all users
**URL**:  
`http://localhost:5000/users/all`

**Screenshot**:  
![GET ALL USERS](./user-testing-screenshots/ALL-USERS.png)

---

## Middleware to Protect the Route and Its Purpose:

### `checkUserFirebaseUid`

**Purpose**:  
Validates the Firebase UID extracted from the `Authorization` header (Bearer token). If no token is provided, the request is treated as from a guest.

**Usage**:  
```js
app.use(checkUserFirebaseUid); // Authenticates user requests
```

### `checkAdminRole`

**Purpose**:  
Checks if the authenticated user has admin privileges or is accessing their own data. If neither condition is met, the request is denied with a "Forbidden" response.

**Usage**:  
```js
app.use(checkAdminRole); // Protects routes that require admin or self-access permissions
```
## Route: GET user by ID (admin only)

**URL**:  
`http://localhost:5000/user/:id`

**Method**:  
`GET`

**Middleware**:  
- `checkUserFirebaseUid`: Validates the Firebase UID from the `Authorization` header (Bearer token).
- `checkAdminRole`: Ensures that the user has admin privileges or is accessing their own data.

**Purpose**:  
Fetches the user data for any user by their ID. This route can only be accessed by an admin or the user themselves.

**Screenshot**:  
![GET SPOECIFIC USER (ADMIN)](./user-testing-screenshots/GET-USER.png)


## Route: POST register user

**URL**:  
`http://localhost:5000/register`

**Method**:  
`POST`

**Middleware**:  
- `validateRequiredFields`: Ensures that the `displayName` and `email` fields are provided in the request body.
- `checkDuplicateUser`: Checks if a user with the same email already exists.
- `checkUserFirebaseUid`: Validates the Firebase UID from the `Authorization` header (Bearer token).

**Purpose**:  
Registers a new user with a Firebase UID and other necessary details like `displayName`, `email`, `photoURL`, and `favorites`. The user role defaults to "user" if not provided.

**Screenshot**:  
![REGISTER USER- already exist](./user-testing-screenshots/user%20already%20exist.png)
![REGISTER USER](./user-testing-screenshots/REGISTER%20USER.png)
![REGISTER USER](./user-testing-screenshots/REGISTER%20USER.png)


## Route: GET all menu items

**URL**:  
`http://localhost:5000/menu`

**Method**:  
`GET`

**Purpose**:  
Fetches all menu items from the database.

**Screenshot**:  
![GET ALL ITEMS](./user-testing-screenshots/get%20all%20items.png)
![GET ALL ITEMS](./user-testing-screenshots/GET%20ALL%20ITEMS%20(2).png)


## Route: POST create new menu item

**URL**:  
`http://localhost:5000/menu/new`

**Method**:  
`POST`

**Middleware**:  
- `validateRequiredFields`: Ensures that the `name`, `basePrice`, and `category` fields are provided in the request body.
- `validateToppings`: Validates that the provided toppings are valid.
- `validateCategory`: Validates that the provided category is valid.
- `checkUserFirebaseUid`: Validates the Firebase UID from the `Authorization` header (Bearer token).
- `checkAdminRole`: Ensures the user has admin privileges or is accessing their own data.

**Purpose**:  
Creates a new menu item with validated data (name, base price, category, etc.). The request must include valid category and topping data.

**Screenshot**:  
![NEW ITEM](./user-testing-screenshots/create%20new%20item.png)
![NEW ITEM](./user-testing-screenshots/ADDING%20ITEM.png)
![NEW ITEM-FAILED](./user-testing-screenshots/CREATE%20ITEM%20-%20CATEGORY%20VALIDATION%20FAILED.png)
![NEW ITEM-FAILED](./user-testing-screenshots/CREATE%20ITEM%20-%20TOPPING%20VALIDATION%20FAILED.png)

## E2E testing
![order](./user-testing-screenshots/order1.png)
![order](./user-testing-screenshots/order2.png)
![cart](./user-testing-screenshots/order-cart.png)
![success](./user-testing-screenshots/order-placed.png)

## Route: GET authenticated user's orders

**URL**:  
`http://localhost:5000/orders/me`

**Method**:  
`GET`

**Purpose**:  
Fetches all orders associated with the authenticated user.

**Middleware**:  
- `checkUserFirebaseUid`: Validates the Firebase UID from the `Authorization` header (Bearer token).
- `checkUserId`: Validates the user through Firebase and attaches the `userId` to the request object (`req.userId`).

**Response**:  
- **Success (200)**: Returns the user's orders.
- **Error (401)**: Returns an error if the user is not authenticated or the Firebase UID is missing.

**Screenshot**:  
![order history](./user-testing-screenshots/order-history2.png)
![order history](./user-testing-screenshots/order-history.png)


## Route: GET all toppings

**URL**:  
`http://localhost:5000/toppings`

**Method**:  
`GET`

**Purpose**:  
Fetches all available toppings from the database.

**Response**:  
- **Success (200)**: Returns a list of all toppings.

**Screenshot**:  
![ALL TOPPINGS](./user-testing-screenshots/GET%20ALL%20TOPPINGS2.png)
![ALL TOPPINGS](./user-testing-screenshots/GET%20ALL%20TOPPINGS.png)

## Route: POST create payment intent

**URL**:  
`http://localhost:5000/payment`

**Method**:  
`POST`

**Purpose**:  
Creates a payment intent using Stripe to initiate the payment process.

**Response**:

**Success (200)**: Returns the clientSecret for the payment intent.

```json

{
  "clientSecret": "pi_1234_secret_5678"
}
```
![PAYMENT INTENT](./user-testing-screenshots/payment%20succesfull2.png)

## Route: POST store successful payment

**URL**:  
`http://localhost:5000/payment/store`

**Method**:  
`POST`

**Purpose**:  
Stores the successful payment information.

![PAYMENT SUCCESFULL](./user-testing-screenshots/payment%20succesfull.png)
