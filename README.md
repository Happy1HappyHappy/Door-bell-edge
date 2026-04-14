# Real-time Doorway Monitoring System (Edge Component)

This component is the edge application for a Real-time Doorway Monitoring System. It captures video frames from a local USB camera or a video file, scales them, and streams them as an RTSP stream to a [MediaMTX](https://github.com/bluenviron/mediamtx) server.

## Features

- **Live RTSP Streaming:** Uses FFmpeg under the hood to push low-latency video to MediaMTX.
- **Dynamic Configuration:** Easily configurated using environment variables or a `.env` file.
- **Local Preview:** Optional UI to locally preview the camera feed with OpenCV.
- **Docker Ready:** Includes a `Dockerfile` for quick deployment.

## Prerequisites

- **Python 3.11+**
- **FFmpeg**: Required in the system path to push the RTSP stream.
- **Make sure you have an RTSP server (like MediaMTX)** running to ingest the stream.

*(Kafka dependencies are also included via `confluent-kafka` for planned frame processing)*

## Installation

### Local Setup

1. **Create and activate a virtual environment:**
   ```bash
   python3 -m venv venv
   source venv/bin/activate
   ```

2. **Install requirements:**
   ```bash
   pip install -r requirements.txt
   ```

3. **Install FFmpeg (if not already installed):**
   ```bash
   # On macOS
   brew install ffmpeg
   ```

### Docker Setup

Alternatively, you can build the docker image:
```bash
docker build -t doorway-edge .
```

## Configuration

Configuration is managed via environment variables. You can create a `.env` file in the main directory:

| Environment Variable | Description | Default |
|----------------------|-------------|---------|
| `CAMERA_SOURCE` | Camera index (e.g., `0`) or file path | `0` |
| `CAMERA_ID` | Unique identifier for your camera | `cam-01` |
| `MEDIAMTX_URL` | MediaMTX publish URL | `rtsp://localhost:8554/cam-01` |
| `FRAME_WIDTH` | Target video width | `1080` (or `640` in `.env`) |
| `FRAME_HEIGHT` | Target video height | `720` (or `640` in `.env`) |
| `FPS` | Target frames per second | `15` (or `10` in `.env`) |
| `SHOW_PREVIEW` | Show live preview window | `true` |

*(Note: `.env` overrides are managed by you running the script with env variables. E.g. using `python-dotenv` if incorporated later or by exporting them)*

## Running the Application

### Locally:
```bash
python camera_producer.py
```

### Via Docker:
```bash
docker run --device=/dev/video0 -e CAMERA_SOURCE=0 doorway-edge
```
*(Make sure to adjust device bindings depending on your OS / hardware)*

## Project Structure
- `camera_producer.py`: Main entry point for capturing and publishing video.
- `requirements.txt`: Python package dependencies.
- `Dockerfile`: Instructions for containerizing the app.
- `.env`: Example configuration.
