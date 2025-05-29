# Spleeter API Server (Render Deployment Guide)

This guide will help you deploy the Spleeter API server (based on [lekoOwO/spleeter-api](https://github.com/lekoOwO/spleeter-api)) to [Render.com](https://render.com/).

---

## 1. **Clone or Upload the Repo**
- If you haven't already, push this directory to your own GitHub repository (e.g., `yourusername/spleeter-api-server`).
- Or, you can use the Render web UI to upload the code directly.

## 2. **Create a New Web Service on Render**
1. Go to [Render Dashboard](https://dashboard.render.com/).
2. Click **"New +" > "Web Service"**.
3. Connect your GitHub repo (or upload the code).
4. Set the root directory to the folder containing the `Dockerfile` (usually `spleeter-api-server`).
5. Render will auto-detect the Dockerfile and set up the build.

## 3. **Configure Service Settings**
- **Environment**: Docker
- **Build Command**: *(leave blank, Dockerfile handles it)*
- **Start Command**: *(leave blank, Dockerfile handles it)*
- **Port**: `80` (default in Dockerfile)
- **Instance Type**: Use a Standard or Starter instance (CPU is fine; GPU not required for Spleeter CPU mode)

## 4. **Environment Variables (Optional)**
You can set these in the Render dashboard:
- `PROCESSES` (default: 4) — Number of worker processes
- `CLEAN_TIME` (default: 3600) — Time in seconds before cleaning up output files

## 5. **Deploy**
- Click **"Create Web Service"** and wait for the build and deploy to finish.
- Once deployed, you'll get a public URL like `https://your-spleeter-api.onrender.com`.

## 6. **API Endpoints**
- `POST /seperate` — Upload audio file and request stem separation
- `POST /status` — Poll for job status
- `GET /download/{uuid}` — Download the resulting zip file

## 7. **Health Check**
Render supports health checks. Add this to your `api.py` for a simple health endpoint:

```python
class Health:
    def on_get(self, req, resp):
        resp.status = 200
        resp.media = {"status": "ok"}

api.add_route('/health', Health())
```

Then, in Render's settings, set the health check path to `/health`.

## 8. **Test Your API**
- Use `curl` or Postman to test the `/seperate` and `/status` endpoints.
- Integrate with your frontend by setting `SPLEETER_API_URL` to your Render URL.

---

## **Troubleshooting**
- Check Render logs for errors.
- Make sure your instance has enough RAM for larger files.
- For advanced usage, see the [original repo](https://github.com/lekoOwO/spleeter-api).

---

## **License**
MIT. See original Spleeter and API repo for details. 