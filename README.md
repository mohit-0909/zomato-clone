# Zomato Clone (Frontend + Microservices)

This repo contains a Vite/React frontend and multiple Node/TypeScript backend services.

## Prerequisites

- **Node.js**: recommended \(LTS\)
- **npm**
- **MongoDB**: local or Atlas connection string
- **Docker**: for RabbitMQ (required by `restaurant`, `utils`, `rider`)

## Services & Ports (defaults)

- **Frontend**: `http://localhost:5173` (Vite may pick `5174` if busy)
- **Auth**: `http://localhost:5000`
- **Restaurant**: `http://localhost:5001`
- **Utils**: `http://localhost:5002`
- **Realtime**: `http://localhost:5004`
- **Rider**: `http://localhost:5005`
- **Admin**: `http://localhost:5006`

These match the frontend service URLs configured in `frontend/src/main.tsx`.

## Install

From the repo root, install dependencies per module:

```bash
cd ./frontend && npm install
cd ../services/auth && npm install
cd ../restaurant && npm install
cd ../utils && npm install
cd ../realtime && npm install
cd ../rider && npm install
cd ../admin && npm install
```

## Environment variables

This repo **intentionally does not commit** any `.env` files. Create/update the following locally.

### Frontend (`frontend/.env`)

```bash
VITE_GOOGLE_CLIENT_ID=your_google_client_id
VITE_STRIPE_PUBLISHABLE_KEY=your_stripe_publishable_key
VITE_INTERNAL_SERVICE_KEY=match_backend_INTERNAL_SERVICE_KEY
```

### Auth service (`services/auth/.env`)

```bash
PORT=5000
MONGO_URI=mongodb://... or mongodb+srv://...
JWT_SEC=your_jwt_secret
GOOGLE_CLIENT_ID=your_google_client_id
GOOGLE_CLIENT_SECRET=your_google_client_secret
```

### Restaurant service (`services/restaurant/.env`)

```bash
PORT=5001
MONGO_URI=mongodb://... or mongodb+srv://...
JWT_SEC=your_jwt_secret
UTILS_SERVICE=http://localhost:5002
REALTIME_SERVICE=http://localhost:5004
INTERNAL_SERVICE_KEY=choose_a_shared_internal_key

RABBITMQ_URL=amqp://admin:admin123@localhost:5672
PAYMENT_QUEUE=payment_event
RIDER_QUEUE=rider_queue
ORDER_READY_QUEUE=order_ready_queue
```

### Utils service (`services/utils/.env`)

```bash
PORT=5002
FRONTEND_URL=http://localhost:5173
RESTAURANT_SERVICE=http://localhost:5001
INTERNAL_SERVICE_KEY=match_restaurant_INTERNAL_SERVICE_KEY

RABBITMQ_URL=amqp://admin:admin123@localhost:5672
PAYMENT_QUEUE=payment_event

# Cloudinary
CLOUD_NAME=your_cloud_name
CLOUD_API_KEY=your_cloud_api_key
CLOUD_SECRET_KEY=your_cloud_secret_key

# Payments
STRIPE_SECRET_KEY=your_stripe_secret_key
RAZORPAY_KEY_ID=your_razorpay_key_id
RAZORPAY_KEY_SECRET=your_razorpay_key_secret
```

### Realtime service (`services/realtime/.env`)

```bash
PORT=5004
JWT_SEC=your_jwt_secret
INTERNAL_SERVICE_KEY=match_restaurant_INTERNAL_SERVICE_KEY
```

### Rider service (`services/rider/.env`)

```bash
PORT=5005
MONGO_URI=mongodb://... or mongodb+srv://...
JWT_SEC=your_jwt_secret
UTILS_SERVICE=http://localhost:5002
REALTIME_SERVICE=http://localhost:5004
RESTAURANT_SERVICE=http://localhost:5001
INTERNAL_SERVICE_KEY=match_restaurant_INTERNAL_SERVICE_KEY

RABBITMQ_URL=amqp://admin:admin123@localhost:5672
RIDER_QUEUE=rider_queue
ORDER_READY_QUEUE=order_ready_queue
```

### Admin service (`services/admin/.env`)

```bash
PORT=5006
MONGO_URI=mongodb://... or mongodb+srv://...
JWT_SEC=your_jwt_secret

# Use the same DB name convention across services (per setup guide)
DB_NAME=Zomato_Clone
```

## RabbitMQ (Docker)

Start RabbitMQ locally (management UI on `15672`):

```bash
docker run -d --name rabbitmq \
  -p 5672:5672 -p 15672:15672 \
  -e RABBITMQ_DEFAULT_USER=admin \
  -e RABBITMQ_DEFAULT_PASS=admin123 \
  rabbitmq:3-management
```

## Run (development)

Open a **separate terminal per module** and run:

```bash
npm run dev
```

Paths to run it from:

- `frontend/`
- `services/auth/`
- `services/restaurant/`
- `services/utils/`
- `services/realtime/`
- `services/rider/`
- `services/admin/`

## Run (no watch mode)

If your OS hits file watch limits (e.g. `EMFILE: too many open files, watch`), run each backend service without `--watch`:

```bash
npm run build
npm start
```

