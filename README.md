# Image Processing API

A RESTful API service built with Go that provides comprehensive image processing capabilities including resize, crop, rotate, blur, grayscale conversion, and sharpening effects.

## Features

- **Image Operations**: Resize, crop, rotate, blur, grayscale, and sharpen
- **Authentication**: Secure JWT-based user authentication
- **Rate Limiting**: Built-in request rate limiting for API protection
- **Batch Processing**: Process multiple images simultaneously
- **Async Processing**: Handle large image batches asynchronously

## Quick Start

### Prerequisites

- Go 1.19 or higher
- PostgreSQL database

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/zahidhasann88/go-image-processing.git
   cd go-image-processing
   ```

2. **Install dependencies**
   ```bash
   go mod tidy
   ```

3. **Set up environment variables**
   
   Create a `.env` file in the root directory:
   ```env
   DATABASE_URL=postgres://user:password@localhost/imgproc?sslmode=disable
   JWT_SECRET=your_secure_jwt_secret_key_here
   PORT=8080
   ```

4. **Set up the database**
   
   Create a PostgreSQL database named `imgproc` and run:
   ```sql
   CREATE TABLE users (
       id SERIAL PRIMARY KEY,
       username VARCHAR(50) UNIQUE NOT NULL,
       password VARCHAR(255) NOT NULL,
       created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
   );
   ```

5. **Run the application**
   ```bash
   go run cmd/server/main.go
   ```

Your API will be available at `http://localhost:8080`

## Development

### Using Makefile

For Windows users, you'll need one of these tools:
- **Git Bash** (Recommended): Install Git for Windows
- **WSL**: Enable Windows Subsystem for Linux
- **Make for Windows**: Download from GnuWin32

Available commands:
```bash
make run      # Start the development server
make test     # Run all tests
make build    # Build the application binary
make clean    # Remove build artifacts
```

### Manual Build

```bash
# Build for current platform
go build -o image-processing-api cmd/server/main.go

# Run the built binary
./image-processing-api
```

## 📚 API Documentation

### Authentication Endpoints

#### Register a New User
```http
POST /register
Content-Type: application/json

{
  "username": "your_username",
  "password": "your_secure_password"
}
```

**Response:**
```json
{
  "message": "User registered successfully"
}
```

#### User Login
```http
POST /login
Content-Type: application/json

{
  "username": "your_username",
  "password": "your_secure_password"
}
```

**Response:**
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

### Image Processing Endpoints

> **Note**: All image processing endpoints require authentication. Include the JWT token in the Authorization header:
> ```
> Authorization: Bearer your_jwt_token
> ```

#### Single Image Upload
```http
POST /upload
Authorization: Bearer your_jwt_token
Content-Type: multipart/form-data

Form Data:
- image: [file] (required)
- resize: "300x200" (optional)
- crop: "100x100+50+50" (optional)  
- rotate: "90" (optional)
- blur: "2.5" (optional)
- grayscale: "true" (optional)
- sharpen: "1.5" (optional)
```

**Response:**
```json
{
  "message": "Image processed successfully",
  "url": "http://localhost:8080/uploads/processed-image-123.jpg"
}
```

#### Batch Image Upload
```http
POST /batch-upload
Authorization: Bearer your_jwt_token
Content-Type: multipart/form-data

Form Data:
- images: [multiple files] (required)
- resize: "300x200" (optional)
- crop: "100x100+50+50" (optional)
- rotate: "90" (optional)
- blur: "2.5" (optional)
- grayscale: "true" (optional)
- sharpen: "1.5" (optional)
```

**Response:**
```json
{
  "message": "Batch processing completed successfully",
  "processed_count": 5,
  "urls": [
    "http://localhost:8080/uploads/processed-image-1.jpg",
    "http://localhost:8080/uploads/processed-image-2.jpg"
  ]
}
```

#### Asynchronous Image Upload
```http
POST /async-upload
Authorization: Bearer your_jwt_token
Content-Type: multipart/form-data

Form Data:
- images: [multiple files] (required)
- resize: "300x200" (optional)
- crop: "100x100+50+50" (optional)
- rotate: "90" (optional)
- blur: "2.5" (optional)
- grayscale: "true" (optional)
- sharpen: "1.5" (optional)
```

**Response:**
```json
{
  "message": "Images are being processed asynchronously",
  "job_id": "job-123456",
  "status": "processing"
}
```

## 🧪 Testing

### Step 1: Register a User
1. Set method to `POST`
2. URL: `http://localhost:8080/register`
3. Headers: `Content-Type: application/json`
4. Body (raw JSON):
   ```json
   {
     "username": "testuser",
     "password": "securepassword123"
   }
   ```

### Step 2: Login and Get Token
1. Set method to `POST`
2. URL: `http://localhost:8080/login`
3. Body (raw JSON):
   ```json
   {
     "username": "testuser",
     "password": "securepassword123"
   }
   ```
4. Copy the `token` from the response

### Step 3: Process an Image
1. Set method to `POST`
2. URL: `http://localhost:8080/upload`
3. Authorization: `Bearer [paste_your_token_here]`
4. Body: `form-data`
   - Key: `image`, Type: `File`, Value: Select your image
   - Key: `resize`, Type: `Text`, Value: `400x300` (optional)
   - Key: `grayscale`, Type: `Text`, Value: `true` (optional)

## ⚙️ Configuration Options

### Environment Variables

| Variable | Description | Default | Required |
|----------|-------------|---------|----------|
| `DATABASE_URL` | PostgreSQL connection string | - | Yes |
| `JWT_SECRET` | Secret key for JWT token signing | - | Yes |
| `PORT` | Server port | 8080 | No |
| `RATE_LIMIT` | Requests per minute per IP | 100 | No |
| `MAX_FILE_SIZE` | Maximum file size in MB | 10 | No |

### Image Processing Parameters

| Parameter | Description | Example |
|-----------|-------------|---------|
| `resize` | New dimensions (width x height) | `300x200` |
| `crop` | Crop area (width x height + x_offset + y_offset) | `100x100+50+50` |
| `rotate` | Rotation angle in degrees | `90`, `180`, `270` |
| `blur` | Blur intensity (0.1 to 10.0) | `2.5` |
| `grayscale` | Convert to grayscale | `true` |
| `sharpen` | Sharpen intensity (0.1 to 5.0) | `1.5` |

---