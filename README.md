# Redis Cache Layer

## 🌟 Overview

This project provides a high-performance, distributed caching layer using Redis to dramatically reduce database load. By intelligently caching frequently accessed data, applications can achieve significantly faster response times and improved scalability. This layer is designed to be a seamless addition to existing systems, acting as a fast-access intermediary.

## 🚀 Key Features

*   **Distributed Caching**: Leverages Redis for a scalable, in-memory data store accessible across multiple application instances.
*   **High Performance**: Optimized for speed, enabling rapid retrieval and storage of cached data.
*   **Database Load Reduction**: Significantly reduces the strain on primary databases by serving cached responses.
*   **Easy Integration**: Simple class-based interface for straightforward implementation.
*   **Configurable Expiry**: Supports setting Time-To-Live (TTL) for cache entries to ensure data freshness.

## 🛠️ Installation & Setup

**Prerequisites:**

*   Python 3.7+
*   A running Redis instance (local or cluster)

**Installation:**

This library requires the `redis` Python package. Install it using pip:

```bash
pip install redis
```

**Configuration:**

To connect to your Redis instance, ensure the `REDIS_CLUSTER_URL` environment variable is set in your production environment. If not set, the application will default to connecting to a local Redis instance at `redis://localhost:6379/0`.

Example environment variable:

```bash
export REDIS_CLUSTER_URL="redis://your-redis-host:6379/0"
```

## 💻 Usage Instructions

To use the `MemoryLayer` for caching, instantiate the class and then use its methods to interact with the cache.

**1. Instantiate the Cache Layer:**

```python
from redis_cache import MemoryLayer

cache = MemoryLayer()
```

**2. Writing Data to Cache:**

The `write_fast` method stores a key-value pair in the cache. The data will automatically expire after 1 hour (3600 seconds) by default.

```python
cache.write_fast("user:123", "{'name': 'Alice', 'email': 'alice@example.com'}")
```

**3. Fetching Data from Cache:**

The `fetch_fast` method retrieves data associated with a given key.

```python
user_data = cache.fetch_fast("user:123")

if user_data:
    print(f"Data found in cache: {user_data}")
else:
    print("Data not found in cache. Fetching from primary source...")
    # Add logic here to fetch from database and then cache it
```

## 🏗️ Architecture Design

The `Redis Cache Layer` is built around a simple yet effective design pattern for distributed caching:

*   **`MemoryLayer` Class**: This is the primary interface for interacting with the Redis cache.
    *   **`__init__`**: Initializes the connection to the Redis cluster. It reads the `REDIS_CLUSTER_URL` environment variable for production configurations, falling back to a local Redis instance if the variable is not set. This ensures flexibility in deployment environments.
    *   **`fetch_fast(key)`**: Provides a highly optimized method to retrieve data from Redis using a specified key. It directly uses the `redis-py` library's `get` command for efficiency.
    *   **`write_fast(key, value)`**: Offers a streamlined way to store data in Redis. The `set` command is used with an `ex` parameter, setting a default expiration time of 1 hour (3600 seconds) to manage cache staleness automatically.

This architecture prioritizes ease of use, performance, and resilience through its reliance on the robust Redis ecosystem and configurable connection parameters.
