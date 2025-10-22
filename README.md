# Ecommerce Microservices API

Complete ecommerce microservices architecture with User Authentication, Products, Shopping Cart, and Orders management.

## Services

| Service | Port | Description |
|---------|------|-------------|
| **User Service** | 3001 | Authentication (Register/Login) |
| **Product Service** | 3002 | Product management (CRUD) |
| **Cart Service** | 3003 | Shopping cart management |
| **Order Service** | 3004 | Order management & processing |
| **MongoDB** | 27017 | Database |

## Quick Start

### Option 1: Docker Compose (Recommended for Production/Testing)

Start all services with one command:

```bash
cd /Users/jeanruiz/Documents/Proyectos/node/ecommerce
docker compose up --build -d
```

This starts:
- ✅ MongoDB database
- ✅ All 4 microservices
- ✅ All services connected to shared database

To stop:
```bash
docker compose down
```

To view logs:
```bash
docker compose logs -f [service-name]
# Example: docker compose logs -f product-service
```

---

### Option 2: Local Development (Fast Development Cycle)

Run services locally with hot-reload. Better for active development!

**Step 1: Start MongoDB in Docker (only MongoDB)**
```bash
cd /Users/jeanruiz/Documents/Proyectos/node/ecommerce
docker compose up mongo -d
```

**Step 2: Install dependencies for each service**
```bash
cd services/user-services && npm install
cd services/product-services && npm install
cd services/cart-services && npm install
cd services/order-service && npm install
```

**Step 3: Run each service in separate terminal**

Terminal 1 - User Service:
```bash
cd services/user-services
npm run dev
# Runs on http://localhost:3001
```

Terminal 2 - Product Service:
```bash
cd services/product-services
npm run dev
# Runs on http://localhost:3002
```

Terminal 3 - Cart Service:
```bash
cd services/cart-services
npm run dev
# Runs on http://localhost:3003
```

Terminal 4 - Order Service:
```bash
cd services/order-service
npm run dev
# Runs on http://localhost:3004
```

**Stop all services:**
```bash
# Kill all Node processes
killall node

# Stop MongoDB
docker compose down
```

---

## API Documentation

### Recommended Testing Order

1. **Register a User** → Get `user_id`
2. **Create a Product** → Get `product_id`
3. **Create Cart** → Add items
4. **Create Order** → Auto-deletes cart
5. **Update Order Status** → Change status
6. **View Orders** → By status or user

---

## Environment Variables

Each service uses `.env` file for configuration:

```
PORT=3001
MONGO_URI=mongodb://admin:admin123@localhost:27017/ecommerce?authSource=admin
JWT_SECRET=change_this_secret (user-service only)
```

All `.env` files are included in `.gitignore` for security.

---

## Postman Collection

Import `postman_collection.json` in Postman for complete API documentation with:
- ✅ Pre-configured endpoints for all services
- ✅ Environment variables (user_id, product_id, order_id, cart_id)
- ✅ Auto-capture of IDs from responses
- ✅ Sample request bodies

**Variables auto-populated:**
- `user_id` - Captured from register/login
- `product_id` - Captured from create product
- `cart_id` - Captured from create cart
- `order_id` - Captured from create order

---

## API Endpoints

### User Service (Port 3001)
- `POST /auth/register` - Register new user
- `POST /auth/login` - Login user (returns JWT token)

### Product Service (Port 3002)
- `GET /products` - Get all products
- `GET /products/:id` - Get product by ID
- `POST /products` - Create product
- `PUT /products/:id` - Update product
- `DELETE /products/:id` - Delete product

### Cart Service (Port 3003)
- `GET /cart/:userId` - Get user's cart
- `POST /cart` - Create/update cart
- `DELETE /cart/:userId` - Delete cart

### Order Service (Port 3004)
- `GET /orders` - Get all orders
- `POST /orders` - Create order (auto-deletes cart)
- `GET /orders/:id` - Get order by ID
- `GET /orders/user/:userId` - Get user's orders
- `GET /orders/status/:status` - Get orders by status
- `PUT /orders/:id/status` - Update order status
- `DELETE /orders/:id` - Delete order

**Order Statuses:** Pending, Processing, Shipped, Delivered, Cancelled

---

## Database Structure

MongoDB Collections:
- **users** - User accounts with hashed passwords
- **products** - Product catalog
- **carts** - Shopping carts (auto-deleted after order)
- **orders** - Order history with status tracking

---

## Architecture

```
┌─────────────┐
│   Client    │
└──────┬──────┘
       │
   ┌───┴───────────┬────────────┬────────────┐
   │               │            │            │
┌──▼──┐        ┌──▼──┐      ┌──▼──┐      ┌──▼──┐
│Auth │        │Prod │      │Cart │      │Order│
│3001 │        │3002 │      │3003 │      │3004 │
└──┬──┘        └──┬──┘      └──┬──┘      └──┬──┘
   │               │            │            │
   └───────────────┴────────────┴────────────┘
                   │
             ┌─────▼─────┐
             │  MongoDB   │
             │   27017    │
             └────────────┘
```

---

## Development Tips

### File Structure
```
services/
├── user-services/       # Authentication service
├── product-services/    # Product management
├── cart-services/       # Shopping cart
└── order-service/       # Order processing
```

### Hot Reload in Development
Using `npm run dev`:
- Changes to `.ts` files auto-reload
- No need to restart service
- Faster development cycle

### Database Reset
```bash
# Clear all data and restart
docker compose down -v  # -v removes volumes
docker compose up -d
```

---

## Troubleshooting

### Port Already in Use
```bash
# Kill all Node processes
killall node

# Or kill specific port (example: 3001)
lsof -i :3001 | grep LISTEN | awk '{print $2}' | xargs kill -9
```

### MongoDB Connection Error
```bash
# Check if MongoDB is running
docker ps | grep mongo

# If not running:
docker compose up mongo -d
```

### Dependencies Missing
```bash
# Install dependencies
npm install

# For all services:
cd services/user-services && npm install
cd services/product-services && npm install
cd services/cart-services && npm install
cd services/order-service && npm install
```

---

## License

This project is part of the ecommerce microservices architecture.
