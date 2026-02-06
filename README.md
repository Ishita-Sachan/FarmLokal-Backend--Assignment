# FarmLokal-Backend--Assignment
A high-performance Node.js backend designed to handle 1,000,000 product records with sub-200ms latency. This project implements a scalable architecture using MySQL for persistence and Redis for high-speed caching and idempotency.

🚀 Setup Instructions

1- Clone the Repository

2- Install Dependencies

3- Environment Configuration Create a .env file in the root directory and add your credentials:

4- Seed the Database (1 Million Records) Ensure MySQL and Redis (Memurai) are running, then run:

5- Start the Server

🏗️ Architecture Overview

The system is built with a focus on high availability and low latency:

1- API Layer: Express.js with TypeScript handles routing and validation.

2- Caching Layer: Redis (Memurai) is used for query result caching and webhook idempotency.

3- Persistence Layer: MySQL managed via Sequelize ORM with optimized indexing.

4- Security: Environment variable management via dotenv to keep credentials secure.

⚡ Caching Strategy

To achieve P95 latency < 200ms, I implemented a multi-layered caching strategy:

1- Query Caching: Product search and category filter results are cached in Redis for 5 minutes.

2- Auth Caching: Simulated OAuth2 tokens are cached for 1 hour to minimize overhead.

3- Webhook Idempotency: Each eventId is cached for 24 hours in Redis using SET NX to prevent duplicate processing.

📈 Performance Optimizations

1- B-Tree Indexing: Composite indexes on category and name columns transform full-table scans (O(N)) into logarithmic searches (O(log N)).

2- Cursor-based Pagination: Uses WHERE id > cursor instead of OFFSET to ensure constant-time performance even on the 100,000th page.

3- Batch Operations: The seeding script uses bulkCreate with a batch size of 10,000, reducing I/O overhead significantly.

4- Connection Pooling: Optimized Sequelize pool settings to handle high concurrent request volumes.

⚖️ Trade-offs Made

1- Eventual Consistency: By caching search results for 5 minutes, there is a small window where newly added products may not appear. This was chosen to prioritize speed and reduce DB load.

2- In-Memory Caching: Redis is used for fast access, but for mission-critical persistence of logs, a secondary disk-based logging system would be required in production.

3- Simplistic Search: I used SQL LIKE queries for this assignment. For more complex "fuzzy" search needs (e.g., spellcheck), integrating ElasticSearch would be the next step.
