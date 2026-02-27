# retail-vision-analytics

Edge-based computer vision platform for retail analytics. Real-time shelf gap detection, queue monitoring, heatmap generation, and product dwell time tracking using on-device AI inference.

## Features

- **Shelf Gap Detection**: Automatically detect out-of-stock items on shelves
- **Queue Monitoring**: Real-time queue length detection with manager alerts at thresholds
- **Product Heatmaps**: Visualize customer dwell zones and high-footfall areas
- **On-Device Inference**: All processing happens at the edge (no raw video sent to cloud)
- **RESTful API**: FastAPI backend for easy integration
- **React Dashboard**: Real-time monitoring and analytics dashboard
- **Privacy-First**: Anonymized processing, no facial recognition

## Tech Stack

- **Backend**: Python 3.9+, FastAPI, PostgreSQL
- **ML/CV**: YOLOv8, OpenCV, NumPy
- **Frontend**: React, TypeScript, Recharts
- **Deployment**: Docker, Docker Compose
- **Edge Devices**: Raspberry Pi, Jetson Nano, x86 with GPU support

## Project Structure

```
retail-vision-analytics/
├── backend/                 # FastAPI backend
│   ├── main.py             # FastAPI app entry point
│   ├── models/             # ML model loading
│   ├── detection/          # Computer vision logic
│   ├── api/                # API routes
│   ├── database/           # PostgreSQL schemas
│   └── requirements.txt    # Python dependencies
├── frontend/               # React dashboard
│   ├── src/
│   ├── public/
│   └── package.json
├── docker-compose.yml      # Development setup
├── Dockerfile              # Backend container
└── README.md
```

## Quick Start

### Prerequisites
- Python 3.9+
- Docker & Docker Compose
- 4GB RAM minimum (8GB+ recommended)
- GPU optional but recommended for edge devices

### Development Setup

```bash
# Clone repo
git clone https://github.com/ramji3030/retail-vision-analytics
cd retail-vision-analytics

# Install Python dependencies
cd backend
pip install -r requirements.txt

# Run backend
python main.py

# In another terminal, setup frontend
cd frontend
npm install
npm start
```

### Docker Deployment

```bash
docker-compose up -d
```

API will be available at `http://localhost:8000`
Dashboard at `http://localhost:3000`

## API Endpoints

### Shelf Gap Detection
```
POST /api/detect/shelf-gaps
Content-Type: multipart/form-data
- file: image or video frame

Response:
{
  "detected_gaps": [
    {"shelf_id": "A1", "position": [100, 200], "confidence": 0.92}
  ],
  "timestamp": "2026-02-26T23:00:00Z"
}
```

### Queue Detection
```
POST /api/detect/queue
Content-Type: multipart/form-data
- file: image or video frame

Response:
{
  "queue_length": 5,
  "alert": true,
  "confidence": 0.88,
  "timestamp": "2026-02-26T23:00:00Z"
}
```

### Analytics
```
GET /api/analytics/heatmap?store_id=STORE_001&date=2026-02-26

Response:
{
  "heatmap_url": "...",
  "dwell_times": {...},
  "footfall_density": {...}
}
```

## Model Details

- **Detection Model**: YOLOv8m fine-tuned on retail shelf and queue datasets
- **Model Size**: ~50MB (quantized)
- **Inference Speed**: 30-60ms per frame on CPU, <10ms on GPU
- **Input**: 640x480 or 1280x720 video/images
- **Output**: Bounding boxes, confidence scores, class labels

## Configuration

Create `.env` file in backend:

```
DATABASE_URL=postgresql://user:password@localhost:5432/retail_analytics
QUEUE_ALERT_THRESHOLD=5
SHELF_GAP_CONFIDENCE=0.85
API_PORT=8000
EDGE_DEVICE=cpu  # or 'cuda' for GPU
```

## Performance Benchmarks

| Device | FPS | CPU Usage | Memory |
|--------|-----|-----------|--------|
| CPU (i5) | 15-20 | 60-80% | 500MB |
| Raspberry Pi 4 | 5-8 | 85-95% | 400MB |
| Jetson Nano | 20-25 | 40-60% | 800MB |
| GPU (RTX 3060) | 60+ | 20-30% | 2GB |

## Privacy & Security

- ✅ No raw video stored
- ✅ No facial recognition
- ✅ On-device processing only
- ✅ Anonymized bounding boxes only sent to analytics
- ✅ HTTPS/TLS for API communication
- ✅ JWT authentication for dashboard

## Roadmap

- [ ] Multi-camera sync and cross-floor analytics
- [ ] Shelf placement optimization recommendations
- [ ] Staff performance metrics (non-invasive)
- [ ] Product-level sales correlation with shelf position
- [ ] Mobile app for store managers
- [ ] ML model auto-retraining pipeline

## Contributing

Contributions welcome! Please:
1. Fork the repo
2. Create a feature branch
3. Commit changes
4. Push to branch
5. Open a Pull Request

## License

MIT License - see LICENSE file

## Contact

Email: ramji3030@example.com
