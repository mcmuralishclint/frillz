# Frillz.lk Backend Server

Production-ready Node.js backend API server for cPanel deployment.

## Features

- ✅ Express.js server
- ✅ Better-SQLite3 database
- ✅ CORS support
- ✅ Environment variables via dotenv
- ✅ Health check endpoint
- ✅ Graceful shutdown
- ✅ Request logging
- ✅ Error handling
- ✅ Memory-safe (no exit code 137)
- ❌ NO Vite
- ❌ NO frontend serving
- ❌ NO Replit dependencies

## Quick Start

```bash
# Install dependencies
npm install

# Configure environment
cp .env.example .env
# Edit .env with your settings

# Start server
npm start
```

## Environment Variables

Required variables in `.env`:

```bash
NODE_ENV=production
PORT=3000
ALLOWED_ORIGINS=https://yourdomain.com
SESSION_SECRET=your-random-secret-here
```

Generate session secret:
```bash
openssl rand -base64 32
```

## API Endpoints

### Health Check
```
GET /health
```

Response:
```json
{
  "status": "ok",
  "timestamp": "2026-01-09T...",
  "environment": "production",
  "database": "connected",
  "uptime": 123.45
}
```

### Status
```
GET /api/status
```

Response:
```json
{
  "message": "API is running",
  "version": "1.0.0"
}
```

## cPanel Deployment

### Step 1: Upload Files

Upload all files to `/home/yourusername/frillz-backend`

### Step 2: Install Dependencies

Via SSH or cPanel Terminal:
```bash
cd ~/frillz-backend
npm install --production
```

### Step 3: Configure Environment

```bash
cp .env.example .env
nano .env  # Edit with your settings
```

Required settings:
- `NODE_ENV=production`
- `PORT=3000` (or cPanel assigned port)
- `ALLOWED_ORIGINS=https://yourdomain.com`
- `SESSION_SECRET=<random-string>`

### Step 4: Setup Node.js App in cPanel

1. Go to cPanel → "Setup Node.js App"
2. Click "Create Application"
3. Configure:
   - **Node.js version**: 22.x
   - **Application mode**: Production
   - **Application root**: `frillz-backend`
   - **Application URL**: Your subdomain (e.g., `api.yourdomain.com`)
   - **Application startup file**: `server.js`
4. Click "Create"

### Step 5: Start Application

Click "Start App" or "Restart App" in cPanel

### Step 6: Verify

Test health endpoint:
```bash
curl https://api.yourdomain.com/health
```

Expected response:
```json
{
  "status": "ok",
  "timestamp": "...",
  "environment": "production",
  "database": "connected",
  "uptime": ...
}
```

## Database

The server uses `better-sqlite3` with the database file `sqlite.db`.

- Database location: Same directory as `server.js`
- Override with `DATABASE_URL` in `.env`
- Assumes existing tables (no auto-migration)
- WAL mode enabled for better performance

## Adding API Routes

Edit `server.js` and add routes after the health check:

```javascript
// Example: Get all products
app.get('/api/products', (req, res) => {
  try {
    const products = db.prepare('SELECT * FROM products').all();
    res.json(products);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

// Example: Create product
app.post('/api/products', (req, res) => {
  try {
    const { name, price } = req.body;
    const stmt = db.prepare('INSERT INTO products (name, price) VALUES (?, ?)');
    const result = stmt.run(name, price);
    res.status(201).json({ id: result.lastInsertRowid });
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});
```

## Troubleshooting

### Server won't start

**Check Node.js version:**
```bash
node --version  # Should be 18+
```

**Check for errors:**
```bash
node server.js
# Look for error messages
```

### CORS errors

**Symptom**: Browser console shows CORS errors

**Solution**:
1. Check `.env` has `ALLOWED_ORIGINS` set
2. Include your frontend domain
3. Restart server

### Database errors

**Symptom**: "Cannot open database" or similar

**Solution**:
1. Ensure `sqlite.db` exists in same directory
2. Check file permissions: `chmod 644 sqlite.db`
3. Verify path in `DATABASE_URL` (if set)

### Memory issues (exit code 137)

**Symptom**: Server crashes with exit code 137

**Solution**:
- Increase memory limit in cPanel Node.js app settings
- Check for memory leaks in your code
- Use `--max-old-space-size` flag if needed

## File Structure

```
backend-build/
├── server.js          # Main server file
├── package.json       # Dependencies
├── .env.example       # Environment template
├── .env               # Your config (create this)
├── sqlite.db          # Database file
├── uploads/           # File uploads directory
└── README.md          # This file
```

## Production Checklist

- [ ] Dependencies installed: `npm install`
- [ ] `.env` file created and configured
- [ ] `ALLOWED_ORIGINS` set to frontend domain
- [ ] `SESSION_SECRET` set to random string
- [ ] Database file (`sqlite.db`) present
- [ ] Server starts locally: `node server.js`
- [ ] Health check works: `curl http://localhost:3000/health`
- [ ] Uploaded to cPanel
- [ ] Node.js app configured in cPanel
- [ ] Application started in cPanel
- [ ] Health check accessible via public URL
- [ ] CORS working (no browser errors)

## Support

For issues:
1. Check server logs in cPanel
2. Test health endpoint
3. Verify environment variables
4. Check database file exists and has correct permissions
