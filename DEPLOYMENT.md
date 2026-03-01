# SpliX Deployment Guide

## CORS Configuration

The application is now configured with proper CORS settings for deployment.

### Backend Configuration

The backend CORS is controlled via environment variables in `backend/.env`:

```bash
# Add your frontend URLs separated by commas
CORS_ORIGINS="http://localhost:5173,https://yourdomain.com,https://www.yourdomain.com"
NODE_ENV="production"
```

**Important**:

- For development, keep localhost URLs
- For production, replace with your actual frontend domain(s)
- Multiple origins are supported (comma-separated)
- Credentials are enabled for cookie/session support
- Allowed methods: GET, POST, PUT, PATCH, DELETE, OPTIONS
- Allowed headers: Content-Type, Authorization

### Frontend Configuration

Update `frontend/.env` with your backend API URL:

```bash
# Development
VITE_API_BASE_URL=http://localhost:3000/api

# Production
VITE_API_BASE_URL=https://api.yourdomain.com/api
```

### Deployment Steps

#### 1. Backend Deployment (e.g., Railway, Render, Fly.io)

1. Set environment variables on your hosting platform:

   ```
   DATABASE_URL=your_supabase_connection_string
   DIRECT_URL=your_supabase_direct_connection
   JWT_SECRET=secure_random_string
   JWT_REFRESH_SECRET=another_secure_random_string
   PORT=3000
   CORS_ORIGINS=https://yourfrontend.com,https://www.yourfrontend.com
   NODE_ENV=production
   ```

2. Build command: `npm run build`
3. Start command: `npm start`

#### 2. Frontend Deployment (e.g., Vercel, Netlify, Cloudflare Pages)

1. Set environment variable:

   ```
   VITE_API_BASE_URL=https://your-backend-url.com/api
   ```

2. Build command: `npm run build`
3. Output directory: `dist`

### Security Notes

- Never commit `.env` files to git (already in `.gitignore`)
- Use `.env.example` files as templates
- Generate strong JWT secrets for production
- Use HTTPS in production
- Restrict CORS origins to your actual frontend domains
- Update Supabase database credentials if needed

### Testing CORS

After deployment, test CORS by making a request from your frontend:

```javascript
fetch("https://your-backend-url.com/api/auth/login", {
  method: "POST",
  credentials: "include",
  headers: {
    "Content-Type": "application/json",
  },
  body: JSON.stringify({ email: "test@example.com", password: "password" }),
});
```

If CORS is configured correctly, you should receive a response without CORS errors.
