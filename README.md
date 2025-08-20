# YouTube API Server

A FastAPI-based server providing API endpoints for extracting and processing YouTube video data, including metadata, captions, and timestamps.

## Features

- Extract video metadata using YouTube's oEmbed API
- Retrieve video captions/transcripts
- Generate timestamped captions
- RESTful API with Swagger/OpenAPI documentation
- Docker support for easy deployment

## Requirements

- Python 3.8+
- FastAPI
- youtube-transcript-api
- Docker (optional)

## Installation

### Using Python

1. Clone the repository:
   ```bash
   git clone https://github.com/chinpeerapat/youtube-api-server.git
   cd youtube-api-server
   ```

2. Create a virtual environment:
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

4. Create a `.env` file from the example:
   ```bash
   cp .env.example .env
   ```

5. Run the server:
   ```bash
   uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
   ```
   
   Alternative method:
   ```bash
   python run.py
   ```

### Using Docker

1. Clone the repository:
   ```bash
   git clone https://github.com/chinpeerapat/youtube-api-server.git
   cd youtube-api-server
   ```

2. Build and start the Docker container:
   ```bash
   docker-compose up -d
   ```

## Usage

Once the server is running, you can access:

- API documentation: http://localhost:8000/docs
- Alternative API documentation: http://localhost:8000/redoc

### API Endpoints

#### 1. Get Video Metadata

```
POST /youtube/video-data
```

Request body:
```json
{
  "url": "https://www.youtube.com/watch?v=dQw4w9WgXcQ"
}
```

Response:
```json
{
  "title": "Video Title",
  "author_name": "Channel Name",
  "author_url": "https://www.youtube.com/channel/...",
  "type": "video",
  "height": 113,
  "width": 200,
  "version": "1.0",
  "provider_name": "YouTube",
  "provider_url": "https://www.youtube.com/",
  "thumbnail_url": "https://i.ytimg.com/vi/..."
}
```

#### 2. Get Video Captions

```
POST /youtube/video-captions
```

Request body:
```json
{
  "url": "https://www.youtube.com/watch?v=dQw4w9WgXcQ",
  "languages": ["en"]
}
```

Response:
```
"Text of the captions..."
```

#### 3. Get Video Timestamps

```
POST /youtube/video-timestamps
```

Request body:
```json
{
  "url": "https://www.youtube.com/watch?v=dQw4w9WgXcQ",
  "languages": ["en"]
}
```

Response:
```json
[
  "0:00 - Caption at the beginning",
  "0:05 - Next caption",
  "0:10 - Another caption"
]
```

## Testing

### Test the API with curl

1. **Start the server** (in one terminal):
   ```bash
   uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
   ```

2. **Test video captions endpoint**:
   ```bash
   curl -X POST http://localhost:8000/youtube/video-captions \
     -H "Content-Type: application/json" \
     -d '{
       "url": "https://www.youtube.com/watch?v=4AwyVTHEU3s",
       "languages": ["en"]
     }'
   ```

3. **Test video timestamps endpoint**:
   ```bash
   curl -X POST http://localhost:8000/youtube/video-timestamps \
     -H "Content-Type: application/json" \
     -d '{
       "url": "https://www.youtube.com/watch?v=4AwyVTHEU3s",
       "languages": ["en"]
     }'
   ```

4. **Test video metadata endpoint**:
   ```bash
   curl -X POST http://localhost:8000/youtube/video-data \
     -H "Content-Type: application/json" \
     -d '{
       "url": "https://www.youtube.com/watch?v=4AwyVTHEU3s"
     }'
   ```

### Test with Python

```python
from app.utils.youtube_tools import YouTubeTools

# Test transcript functionality
test_url = 'https://www.youtube.com/watch?v=4AwyVTHEU3s'

# Get captions
captions = YouTubeTools.get_video_captions(test_url, ['en'])
print(f"Captions: {captions[:200]}...")

# Get timestamps
timestamps = YouTubeTools.get_video_timestamps(test_url, ['en'])
print(f"First 3 timestamps: {timestamps[:3]}")

# Get video metadata
metadata = YouTubeTools.get_video_data(test_url)
print(f"Video title: {metadata['title']}")
```

## Project Structure

```
youtube-api-server/
├── app/
│   ├── __init__.py
│   ├── main.py                # FastAPI application initialization
│   ├── models/                # Pydantic models
│   │   ├── __init__.py
│   │   └── youtube.py
│   ├── routes/                # API routes
│   │   ├── __init__.py
│   │   └── youtube.py
│   └── utils/                 # Utility functions
│       ├── __init__.py
│       └── youtube_tools.py
├── .env.example               # Example environment variables
├── requirements.txt           # Python dependencies
├── Dockerfile                 # Docker configuration
└── docker-compose.yml         # Docker Compose configuration
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the LICENSE file for details.