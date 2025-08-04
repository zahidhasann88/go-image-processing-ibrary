# Image Processing API

RESTful API service built with Go for comprehensive image processing operations.

## 🚀 Features

- **Operations**: Resize, crop, rotate, blur, grayscale, sharpen
- **Security**: JWT authentication, rate limiting
- **Processing**: Single, batch, and async image processing

## Quick Setup

### Prerequisites
- Go 1.19+
- PostgreSQL

### Install & Run
```bash
git clone https://github.com/zahidhasann88/go-image-processing.git
cd go-image-processing
go mod tidy

# Create .env file
echo "DATABASE_URL=postgres://user:password@localhost/imgproc?sslmode=disable
JWT_SECRET=your_secure_jwt_secret
PORT=8080" > .env

# Setup database
createdb imgproc
psql imgproc -c "CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);"

# Run
go run cmd/server/main.go
```

Server runs at `http://localhost:8080`

## 📖 API Usage

### Authentication
```bash
# Register
curl -X POST http://localhost:8080/register \
  -H "Content-Type: application/json" \
  -d '{"username": "user", "password": "pass123"}'

# Login (get token)
curl -X POST http://localhost:8080/login \
  -H "Content-Type: application/json" \
  -d '{"username": "user", "password": "pass123"}'
```

### Image Processing
```bash
# Single image
curl -X POST http://localhost:8080/upload \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -F "image=@/path/to/image.jpg" \
  -F "resize=300x200" \
  -F "grayscale=true"

# Batch processing
curl -X POST http://localhost:8080/batch-upload \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -F "images=@image1.jpg" \
  -F "images=@image2.jpg" \
  -F "blur=2.5"

# Async processing
curl -X POST http://localhost:8080/async-upload \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -F "images=@image1.jpg" \
  -F "images=@image2.jpg"
```

## 🔧 Processing Parameters

| Parameter | Description | Example |
|-----------|-------------|---------|
| `resize` | Width x Height | `300x200` |
| `crop` | Width x Height + X + Y offset | `100x100+50+50` |
| `rotate` | Degrees (90, 180, 270) | `90` |
| `blur` | Intensity (0.1-10.0) | `2.5` |
| `grayscale` | Convert to grayscale | `true` |
| `sharpen` | Intensity (0.1-5.0) | `1.5` |

## 🛠️ Development

```bash
# Using Makefile (requires Make/Git Bash on Windows)
make run      # Start server
make test     # Run tests
make build    # Build binary

# Manual build
go build -o api cmd/server/main.go
./api
```

## 📋 Environment Variables

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `DATABASE_URL` | ✅ | - | PostgreSQL connection |
| `JWT_SECRET` | ✅ | - | JWT signing key |
| `PORT` | ❌ | 8080 | Server port |
| `RATE_LIMIT` | ❌ | 100 | Requests/min per IP |
| `MAX_FILE_SIZE` | ❌ | 10 | Max file size (MB) |

## 🧪 Quick Test with Postman

1. **Register**: `POST /register` with `{"username": "test", "password": "test123"}`
2. **Login**: `POST /login` → Copy the `token`
3. **Upload**: `POST /upload` with `Authorization: Bearer TOKEN` and form-data `image` file

---

**Built with Go** • [Report Issues](../../issues)