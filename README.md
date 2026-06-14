  # CloudStore — Distributed File Storage System
  https://github.com/yash-khapre8/SystemDesign_Cloudstore

A distributed file storage system design with chunked uploads, replication, metadata services, and fault tolerance — inspired by Dropbox/Google Drive architecture.

This project was built as part of a System Design final examination, simulating how a cloud-based file storage platform (similar to Google Drive/Dropbox) handles file uploads, downloads, synchronization, sharing, versioning, and scaling to millions of users.

---

## Project Overview

CloudStore is designed around a **microservices architecture**, where each service owns its responsibility and its own database — avoiding a single point of failure or bottleneck as the system scales.

**Core features implemented:**
- Chunked file upload & download (large files split into fixed-size chunks)
- Content-based deduplication using SHA-256 chunk hashing
- File versioning (every update creates a new version, old chunks reused via dedup)
- Distributed storage simulation with replication (fault tolerant — survives node failures)
- File sharing with role-based access control (owner / read / write)
- Real-time sync notifications, with offline backlog delivery for reconnecting devices
- Per-user storage quota enforcement
- Database-per-service design (Metadata DB, Users & Devices DB, Chunk DB)

**Architecture components:**

| Component | Responsibility |
|---|---|
| API Gateway | Entry point, routes requests, maintains client sessions for real-time sync |
| Load Balancer | Distributes traffic across gateway instances |
| Metadata Service | Stores file names, paths, versions, ownership, permissions |
| Chunk Service | Splits files into chunks, hashes for dedup, tracks chunk-to-version mapping |
| Block Service / Storage Nodes | Physically stores chunks, replicated across multiple nodes |
| Users & Devices Service | Manages user accounts, storage quota, registered devices |
| Notification Service | Pushes sync events to clients; queues backlog for offline devices |
| Billing Service | Tracks subscription/billing info per user |  



---

## Tech Stack

- **Language:** Python 3.10+
- **Database:** SQLite (one database per service, simulating the database-per-service principle)
- **Storage simulation:** Local filesystem (multiple folders simulating distributed storage nodes)
- **Hashing:** SHA-256 (for chunk integrity & deduplication)
- **Environment:** Google Colab / Jupyter Notebook

---

## Dependencies

This project uses **only the Python standard library** — no external packages need to be installed.

```
os
sqlite3
hashlib
shutil
uuid
time
```

---

## Setup Instructions

1. Clone this repository:
   ```bash
   git clone https://github.com/<your-username>/cloudstore-distributed-file-system.git
   cd cloudstore-distributed-file-system
   ```

2. Open `CloudStore_Simulation.ipynb` in **Google Colab** or **Jupyter Notebook**.
   - In Colab: `File → Upload notebook` and select the `.ipynb` file.

No additional installation or environment setup is required — the notebook runs out of the box.

---

## Execution Steps

The notebook is structured so every component and feature can be inspected and run individually:

1. **Run cells 1–7 first (top to bottom)** — these define the configuration and all five service classes (`StorageCluster`, `MetadataService`, `ChunkService`, `UserDeviceService`, `NotificationService`) plus the `CloudStore` facade (API Gateway).

2. **Run Task 0** — resets all databases/storage and initializes two test users (Alice, Bob) with registered devices.

3. **Run Tasks 1–9 individually**, in any order after Task 0 — each task demonstrates one specific requirement:

   | Task | Demonstrates |
   |---|---|
   | 1 | Chunked file upload + replication across storage nodes |
   | 2 | Offline device reconnect & notification backlog delivery |
   | 3 | File sharing & access control |
   | 4 | Chunked download + data integrity verification |
   | 5 | File versioning & chunk deduplication |
   | 6 | Permission enforcement (rejecting unauthorized writes) |
   | 7 | Fault tolerance — file remains available after 2 storage node failures |
   | 8 | Storage quota enforcement |
   | 9 | Inspecting raw database records & physical chunk distribution across nodes |

Each task prints clear, labeled output showing exactly which service handled the request and what happened — making it easy to map code execution back to the architecture diagram during evaluation.

---

