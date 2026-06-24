# Vastraa - Premium E-commerce Ecosystem

Vastraa is a production-ready, feature-rich E-commerce platform designed to bridge the gap between buyers, sellers, and administrators. Built with a modern tech stack, it emphasizes security, scalability, and AI-enhanced user experiences.

## Links

- **Live Demo**: [https://vastraa-05dq.onrender.com](https://vastraa-05dq.onrender.com/)

## Image

<img width="3150" height="2450" alt="Vastra" src="https://github.com/user-attachments/assets/dd4f6182-2282-4f70-a24e-66864098fb7b" />


## 1. Project Overview

Vastraa is a comprehensive marketplace solution that caters to three distinct user groups:
- **Buyers (Customers):** Individuals looking for high-quality apparel with a seamless shopping and support experience.
- **Sellers (Vendors):** Small to large-scale businesses seeking a platform to manage inventory, track orders, and grow their brand.
- **Admins:** Platform operators who ensure quality control, resolve disputes, and monitor platform health.

**Problem Solved:** Traditional e-commerce platforms often lack integrated AI support and robust vendor management in a single lightweight package. Vastraa solves this by providing AI-powered product descriptions for sellers and an intelligent chatbot for buyers, all while maintaining strict security standards.

---

## 2. Core Features

- **Multi-User Ecosystem**: Dedicated portals and tailored dashboards for Buyers (shopping & tracking), Sellers (inventory management), and Administrators (platform moderation).
- **AI-Powered Assistance**: Integrated "Vastraa Assistant" chatbot for buyer recommendations and an AI-driven product description generator for sellers.
- **Product & Order Management**: Comprehensive catalog filtering, full-featured CRUD operations for vendors, and real-time order status tracking.
- **Robust Security Framework**: Secure cookie-based JWT authentication, CSRF double-cookie submit protection, and role-based access control (RBAC).
- **Media Delivery & Storage**: Performance-optimized image uploads and real-time media transformations powered by ImageKit.

---

## 3. Tech Stack

- **Frontend Framework**: [React 19 (Vite)](https://react.dev/) with [Zustand](https://zustand-demo.pmnd.rs/) for state management and [Framer Motion](https://www.framer.com/motion/) for animations.
- **Backend Framework**: [Node.js](https://nodejs.org/) / [Express.js 5](https://expressjs.com/) hosting a modular monolithic API.
- **Database & Caching**: [MongoDB](https://www.mongodb.com/) via [Mongoose ODM](https://mongoosejs.com/) and [Redis](https://redis.io/) caching layer.
- **Styling & UI**: [Tailwind CSS 4.0](https://tailwindcss.com/) for streamlined modern utility-first styling.
- **AI & Storage Integrations**: [ImageKit.io](https://imagekit.io/) for media hosting and [OpenAI / Gemini SDK](https://openai.com/) for description generation.

---

## 4. Architecture Overview

Vastraa follows a modern **Monolithic API** with a **Decoupled Frontend** architecture.

- **Frontend Structure:** Component-based architecture utilizing React Context and Zustand for state. Routes are protected via a robust Role-Based Access Control (RBAC) system.
- **Backend Structure:** Modular MVC (Model-View-Controller) pattern with a dedicated Service layer for cross-cutting concerns (Mail, Tokens, AI).
- **Authentication Flow:**
    1. User logs in/registers.
    2. Server issues a JWT (JSON Web Token) stored in an **HTTP-only cookie**.
    3. Subsequent requests use the cookie for identity verification.
- **Request-Response Lifecycle:**
    - Middleware Layer (Helmet -> CORS -> Rate Limiting -> CSRF Protection)
    - Authentication/Authorization check
    - Request Validation (Joi)
    - Controller Execution -> Service Layer -> Database
    - Error Handling Middleware (Unified response format)

---

## 5. Folder Structure

```text
Vastraa/
├── backend/
│   ├── config/             # DB (MongoDB), Redis, Admin Account Sync
│   ├── controllers/        # Business logic for all modules
│   ├── middleware/         # Auth (Admin/Seller/Buyer), Multer, CSRF, Error Handler
│   ├── models/             # Mongoose Schemas (User, Product, Order, Cart, etc.)
│   ├── routes/             # Express Route definitions
│   ├── schemas/            # Joi Validation schemas
│   ├── services/           # Reusable services (Mail, ImageKit, AI, Tokens)
│   ├── utils/              # Logger and helper utilities
│   └── server.js           # API Entry point
└── frontend/
    ├── src/
    │   ├── assets/         # Global styles, fonts, and static images
    │   ├── components/     # UI Components (Navbar, Footer, Modals, Cards)
    │   ├── context/        # legacy Context API
    │   ├── pages/          # View components (Admin, Seller, Buyer sub-folders)
    │   ├── store/          # Zustand State Management
    │   ├── utils/          # Formatting and UI helpers
    │   └── App.jsx         # Main routing and global providers
```

---

## 6. Environment Variables

Create a `.env` file in the `backend` directory.

```env
# Database & Server
PORT=4000
MONGODB_URI=your_mongodb_connection_string

# Security
JWT_SECRET=your_jwt_secret
REFRESH_TOKEN_SECRET=your_refresh_token_secret
CSRF_SECRET=your_csrf_secret_string
ALLOWED_ORIGINS=http://localhost:5173

# Third-Party APIs
IMAGEKIT_PUBLIC_KEY=your_public_key
IMAGEKIT_PRIVATE_KEY=your_private_key
IMAGEKIT_URL_ENDPOINT=https://ik.imagekit.io/your_id

OPENAI_API_KEY=your_openai_or_gemini_key
OPENAI_BASE_URL=https://generativelanguage.googleapis.com/v1beta/openai/
OPENAI_MODEL=gemini-3-flash-preview

# Email (Nodemailer)
EMAIL_USER=your_email@gmail.com
EMAIL_PASS=your_app_password

# Redis
UPSTASH_REDIS_REST_URL=your_redis_url
UPSTASH_REDIS_REST_TOKEN=your_redis_token

# Default Admin (Auto-created on first run)
ADMIN_EMAIL=admin@vastraa.com
ADMIN_PASSWORD=VastraaAdmin123
```

---

## 7. Installation & Setup

### Prerequisites
- Node.js (v18+)
- MongoDB Atlas account or local instance
- ImageKit account
- Redis instance (Upstash recommended)

### Backend Setup
1. Navigate to the backend folder: `cd backend`
2. Install dependencies: `npm install`
3. Configure your `.env` file based on the template above.
4. Start development server: `npm run dev`

### Frontend Setup
1. Navigate to the frontend folder: `cd frontend`
2. Install dependencies: `npm install`
3. Start Vite server: `npm run dev`

---

## 8. Security Implementation

- **Authentication:** Cookie-based JWT implementation. Cookies are marked `HttpOnly` and `Secure` to prevent XSS.
- **CSRF Protection:** Implemented using the "Double Submit Cookie" pattern via `csrf-csrf`. Tokens are required for all state-changing requests (POST, PUT, DELETE).
- **Rate Limiting:** 
    - **Global:** 300 requests per 15 minutes per IP.
    - **Auth:** 20 attempts per 15 minutes for sensitive routes (Login, Register, OTP).
- **Ownership Checks:** Backend middleware ensures that sellers can only modify their own products and buyers can only view their own orders.
- **Input Validation:** Strict schema validation using **Joi** for every incoming request body.

---

## 9. API Documentation

| Method | Endpoint | Description |
| :--- | :--- | :--- |
| **AUTH** | | |
| POST | `/api/user/register` | Register new user (Buyer/Seller) |
| POST | `/api/user/login` | Login and receive JWT cookie |
| GET | `/api/user/logout` | Clear auth cookies |
| **PRODUCT** | | |
| GET | `/api/product/list` | List all available products |
| POST | `/api/product/add` | (Seller) Add new product |
| **CART/ORDER**| | |
| POST | `/api/cart/add` | Add item to buyer cart |
| POST | `/api/order/place` | Convert cart items to order |
| **ADMIN** | | |
| GET | `/api/admin/stats` | Platform metrics dashboard |
| POST | `/api/admin/ban/:id` | Suspend a user account |

---

## 10. Deployment Instructions

### Backend
1. Ensure `NODE_ENV=production` is set in the environment.
2. Build command is not required for Node; start with `npm start`.
3. Recommended platforms: **Render**, **Railway**, or **AWS Elastic Beanstalk**.

### Frontend
1. Run `npm run build` to generate the `dist` folder.
2. Deploy the static `dist` folder to **Vercel** or **Netlify**.
3. Ensure the `VITE_BACKEND_URL` (if applicable) points to your deployed API.

---

## 11. Future Improvements

- [ ] **Payment Gateway:** Integration with Stripe or Razorpay for online transactions.
- [ ] **Multi-Currency:** Support for global shopping with real-time currency conversion.
- [ ] **PWA Support:** Offline access and mobile-app like experience.
- [ ] **Advanced AI:** Visual search (upload image to find similar clothes).

---

## 12. Contributing

1. Fork the Project.
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`).
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`).
4. Push to the Branch (`git push origin feature/AmazingFeature`).
5. Open a Pull Request.

---

*Designed & developed [Swagat Gharat](https://github.com/swagatgharat)*
