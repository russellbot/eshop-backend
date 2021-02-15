## Backend of ez-shop

## Deployed URL
to do


### Description
API was created using Node.js and express. The database Mongodb in conjuction with the library mongoose. Testing was done with the application Postman. Product images are handled using Multer middleware. User authentication and admin authentication was done using simple JSON web tokens. Five models were used to store information in separate mongo databases. The user model is used to save user login information and address. The product model stores all information about a product in the store. The category model stores products into a category such as Home and Garden. The order and order-item models were created to store information from each order created by a user. 

## CRUD Routes

| URL | Method | Description |
|-----|--------|-------------|
| "/products" | GET    | List all of the products in the DB |
| "/products | POST | Create a new product. Admin only |
| "/products/:id" | GET |  Get a product by id |
| "/products/:id" | PUT |  Edit a product by id. Admin only |
| "/products/:id" | DELETE | Delete a product by id. Admin only |
| "/products/get/count" | GET | Get count of total products |
| "/products/get/featured/:count" | GET | Get a list of featured products. Enter parameter count. |
| "/products/gallery-images/:id" | PUT | Add images to a product by id. Admin only |
| "/users" | GET | Get a list of users. Admin only |
| "/users/:id" | GET | Get a user by id. Admin only |
| "/users" | POST | Create a new user. Admin only |
| "/users/login" | POST | Log in using user email and password |
| "/users/register" | GET | Register a new user |
| "/users/get/count" | GET | Get a count of total users. Admin only |
| "/users/:id" | DELETE | Delete a user by id. Admin only |
| "/categories" | GET | Get a list of categories |
| "/categories/:id" | GET | Get a category by id |
| "/categories" | POST | Create a new category. Admin only |
| "/categories/:id" | PUT | Edit a category by id. Admin only |
| "/categories/:id" | DELETE | Delete a category by id. Admin only |
| "/orders" | GET | Get a list of orders |
| "/orders/:id" | GET | Get an order by id |
| "/orders" | POST | Create a new order |
| "/orders/:id" | PUT | Edit an order by id. Admin only |
| "/orders/:id" | DELETE | Delete an order by id. Admin only |
| "/orders/get/totalsales" | GET | Get total sales in dollars |
| "/orders/get/count" | GET | Get total count of orders |
| "/orders/get/userorders/:userid" | GET | Get a list of orders by user id |


### Model
```
const userSchema = mongoose.Schema({
    name: {
        type: String,
        required: true,
    },
    email: {
        type: String,
        required: true,
    },
    passwordHash: {
        type: String,
        required: true,
    },
    phone: {
        type: String,
        required: true,
    },
    isAdmin: {
        type: Boolean,
        default: false,
    },
    street: {
        type: String,
        default: ''
    },
    apartment: {
        type: String,
        default: ''
    },
    zip: {
        type: String,
        default: ''
    },
    city: {
        type: String,
        default: ''
    },
    country: {
        type: String,
        default: ''
    }
});
```

```
const productSchema = mongoose.Schema({
    name: {
        type: String,
        required: true,
    },
    description: {
        type: String,
        required: true
    },
    richDescription: {
        type: String,
        default: ''
    },
    image: {
        type: String,
        default: ''
    },
    images: [{
        type: String
    }],
    brand: {
        type: String,
        default: ''
    },
    price: {
        type: Number,
        default: 0
    },
    category: {
        type: mongoose.Schema.Types.ObjectId,
        ref: 'Category',
        required: true
    },
    countInStock: {
        type: Number,
        required: true,
        min: 0,
        max: 255
    },
    rating: {
        type: Number,
        default: 0
    },
    numReviews: {
        type: Number,
        default: 0
    },
    isFeatured: {
        type: Boolean,
        default: false
    },
    dateCreated: {
        type: Date,
        default: Date.now
    }
})
```

```
const categorySchema = mongoose.Schema({
    name: {
        type: String,
        required: true
    },
    icon: {
        type: String,
    },
    color: {
        type: String,
    }
})
```

```
const orderSchema = mongoose.Schema({
    orderItems: [{
        type:mongoose.Schema.Types.ObjectId,
        ref: 'OrderItem',
        required: true
    }],
    shippingAddress1: {
        type: String,
        required: true
    },
    shippingAddress2: {
        type: String
    },
    city: {
        type: String,
        required: true
    },
    zip: {
        type: String,
        requred: true
    },
    country: {
        type: String,
        required: true
    },
    phone: {
        type: String,
        required: true
    },
    status: {
        type: String,
        required: true,
        default: 'Pending'
    },
    totalPrice: {
        type: Number
    },
    user: {
        type: mongoose.Schema.Types.ObjectId,
        ref: 'User'
    },
    dateOrdered: {
        type: Date,
        default: Date.now
    }
})
```

```
const orderItemSchema = mongoose.Schema({
    quantity: {
        type: Number,
        required: true
    },
    product: {
        type: mongoose.Schema.Types.ObjectId,
        ref: 'Product'
    }
})
```