# Uptime Robot Support for Open Design

Open Design daemon exposes a health check endpoint at `/api/health` (or you can use the main port on Render).

## Render + Uptime Robot Setup

### 1. Get your Render service URL
- After deployment succeeds, Render provides: `https://open-design-web.onrender.com`

### 2. Configure Uptime Robot

**In Uptime Robot Dashboard:**

1. Click **Add Monitor**
2. Select **HTTP(S)** monitor type
3. Fill in:
   - **Friendly Name**: `Open Design Web`
   - **URL**: `https://open-design-web.onrender.com/api/health`
   - **Monitor Interval**: Every 5 minutes
   - **Timeout**: 30 seconds
   - **HTTP Method**: GET

4. **Optional: Add Auth Header** (if you set OD_API_TOKEN):
   - Click **Advanced Settings**
   - Add Custom Header:
     ```
     Authorization: Bearer YOUR_OD_API_TOKEN_HERE
     ```

5. **Alerts**: Choose notification (Email, Slack, Discord, etc.)
6. Click **Create Monitor**

### 3. Verify Health Check Works

Test manually:
```bash
curl -v https://open-design-web.onrender.com/api/health
```

Expected response: `200 OK` with JSON health data

### 4. Prevent Render Free Tier Spin-Down

Uptime Robot pings your service every 5 minutes, which **keeps free instances warm** and prevents auto-shutdown. This is the main benefit for Render free tier users.

## Alternative: Render Native Health Checks

If you upgrade to a paid Render tier, you can also use Render's built-in health checks:
1. Go to **Settings** → **Health Check**
2. Set **Health Check Path**: `/api/health`
3. Render will check every 10 seconds

## Monitoring Tips

- **Expected response time**: 50-200ms for `/api/health`
- **Uptime history**: Uptime Robot keeps 90-day logs
- **Incidents**: Get alerts when response time degrades or service is down
- **Status page**: Uptime Robot provides a public status page URL you can share
