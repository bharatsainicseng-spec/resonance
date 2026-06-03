# Dynamic Configuration Management System

A simplified version of Spring Cloud Config built with the MERN stack and Socket.io.

## Features
- **Hierarchical Overrides**: Global → Environment → Client logic.
- **Real-time Updates**: Instant notifications to all connected clients when a config changes.
- **File Watcher**: Automatically syncs JSON files in the `/configs` folder to the database.
- **Admin Dashboard**: Secure management interface for viewing and editing configurations.
- **JWT Authentication**: Protected routes for administrative actions.

## Tech Stack
- **Backend**: Node.js, Express.js, MongoDB, Socket.io, Chokidar
- **Frontend**: React.js, Tailwind CSS, Axios, Lucide React

## Setup Instructions

### Prerequisites
- Node.js (v18+)
- MongoDB running locally or a MongoDB Atlas URI

### 1. Backend Setup
1. Open a terminal in the `/backend` folder.
2. Install dependencies: `npm install`
3. Configure environment variables in `.env`:
   ```env
   PORT=5000
   MONGODB_URI=mongodb://localhost:27017/config-mgmt
   JWT_SECRET=your_jwt_secret_key
   ```
4. Start the server: `npm run dev` (or `node server.js`)

### 2. Frontend Setup
1. Open a terminal in the `/frontend` folder.
2. Install dependencies: `npm install`
3. Start the React dev server: `npm run dev`
4. Access the dashboard at `http://localhost:3000`

### 3. File Watcher Usage
- Create or modify JSON files in the root `/configs` folder.
- **File Format**: `clientId_environment_version.json` (e.g., `user-service_dev_1.0.0.json`).
- The system will detect changes and update the database automatically.

## API Endpoints

### Public
- `GET /api/configs?clientId=X&environment=Y&version=Z`: Get merged configuration.

### Administrative (Requires JWT)
- `POST /api/auth/register`: Create a new admin.
- `POST /api/auth/login`: Authenticate and get token.
- `GET /api/configs/all`: List all configurations.
- `POST /api/configs/update`: Manually update a configuration.

## Hierarchical Logic Example
1. `global_global_1.0.0.json` (Base settings)
2. `global_prod_1.0.0.json` (Overrides global for Production)
3. `clientA_prod_1.0.0.json` (Overrides global + prod for Client A)

When requesting `clientId=clientA&environment=prod`, the system deep-merges them in that order.
