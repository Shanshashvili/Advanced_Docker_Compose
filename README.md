# Advanced Docker Compose with Networking

## Scenario: 
- Create a docker-compose.yml file for the following setup:
  -	A backend service running Python Flask.
  - A frontend service running NGINX to serve static files and reverse proxy requests to the backend.

## Requirements:
-	Both services should run in the same Docker network.
-	The backend service should expose port 5000.
-	Configure the frontend service to forward API requests to the backend.

## Deliverables:
-	The docker-compose.yml file.
-	Commands to start and verify the setup (docker-compose up).
-	A brief explanation of how networking is configured between services.

---


## Steps of Deliverables:

### **The docker-compose.yml file**
[docker-compose.yml](docker-compose.yml)

--
### **Commands to Start and Verify the Setup**

1. **Build and Start the Containers**
   ```bash
   docker compose up --build -d
   ```

2. **Verify Running Containers**
   Check that both containers are running:
   ```bash
   docker ps
   ```

3. **Test the Frontend and API Proxy**
   - use `curl` to verify the frontend is accessible:
     ```bash
     curl http://<host_ip>/
     ```
   - Test the backend API via the frontend proxy:
     ```bash
     curl http://<host_ip>/api/
     ```
   note: Replace `<host_ip>` with your machine’s IP.

--
### **Explanation of Networking**

1. **Docker Network:**
   - Both services are connected to a shared `app-network` created with the `bridge` driver. This enables communication between containers using their service names (`flask-backend` and `nginx-frontend`) as hostnames.

2. **Backend Communication:**
   - The Flask backend is exposed on port `5000`. It handles API requests from the frontend service via the `proxy_pass` directive in NGINX.

3. **Frontend Proxying:**
   - NGINX serves static files and forwards requests with the `/api/` prefix to the backend service (`flask-backend`) over the `app-network`. This configuration allows seamless routing between the frontend and backend.
