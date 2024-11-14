---
theme: default
background: https://source.unsplash.com/collection/94734566/1920x1080
class: 'text-center'
highlighter: shiki
lineNumbers: false
drawings:
  persist: false
transition: slide-left
title: Redis Intensive Training
---

# Art of Redis Intensive Training
One-Day Course (9:00 - 16:00)

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>

---
layout: default
---

# Course Overview

- 🐳 Redis Setup with Docker
- 🏗 Architecture & Cache Strategies
- 💻 Basic Commands & Data Structures
- 🚀 Real-world Applications
  - Session Management
  - Leaderboard System
  - Real-time Sales Analytics
  - Concert Ticket Booking
  - Multiplayer Quiz Game

---
layout: default
---

<div class="container">

<div class="left-column">

# Prerequisites

- Docker Desktop installed
- Basic database knowledge
- PostgreSQL fundamentals
- Text editor/IDE
- Git
</div>

<div class="right-column">

# Schedule

- 9:00 - Setup & Intro
- 9:45 - Architecture
- 10:45 - Basic Commands
- 13:00 - Real Use Cases
- 16:00 - Wrap-up
</div>

</div>
---
layout: cover
background: https://source.unsplash.com/collection/94734566/1920x1080
---

# Part 1: Setup & Docker

---
layout: default
---

# Redis with Docker

```yaml {all|1|2-3|4-8|all}
version: '3.8'
services:
  redis:
    image: redis:latest
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data
```

- Simple setup with Docker Compose
- Persistent storage
- Standard port mapping
- Latest Redis version

---
layout: default
---

# First Steps

```bash {all|1|2|3|all}
# Start Redis container
docker-compose up -d

# Connect to Redis CLI
docker exec -it redis_container redis-cli
```

Try your first commands:
```bash
SET mykey "Hello Redis"
GET mykey
```

---
layout: section
---

# Part 2: Architecture & Cache Strategies

---
layout: default
---

# Redis Architecture

<div class="grid grid-cols-2 gap-4">
<div>

## Single-threaded
- Event-driven
- Fast operations
- No context switching

</div>
<div>

## In-memory
- Quick access
- Volatile by default
- Persistence options

</div>
</div>

---
layout: default
---

# Cache Strategies

- Cache-aside
- Write-through
- Write-behind
- Refresh-ahead

---
layout: section
---

# Part 3: Basic Commands & Data Structures

---
layout: default
---

# String Operations

```bash {all|1-2|3-4|5-6|all}
# Basic operations
SET user:1 "John Doe"
GET user:1

# With expiration
SET token:123 "xyz" EX 3600

# Increment/Decrement
INCR visits:page1
```

---
layout: default
---

# Lists and Sets

```bash {all|1-3|5-7|all}
# Lists
LPUSH notifications "New message"
LRANGE notifications 0 -1

# Sets
SADD tags "redis" "cache" "nosql"
SMEMBERS tags
```

---
layout: default
---

# Sorted Sets (Leaderboard Example)

```bash {all|1-2|4-5|7|all}
# Add scores
ZADD leaderboard 100 "player1" 80 "player2" 95 "player3"

# Get top 3 players
ZREVRANGE leaderboard 0 2 WITHSCORES

# Get player rank
ZREVRANK leaderboard "player1"
```

---
layout: section
---

# Part 4: Real-World Applications

---
layout: default
---

# Session Management

```bash {all|1-2|4-5|7-8|all}
# Store session
SET session:123 "{user: 1, role: 'admin'}" EX 3600

# Check session
GET session:123

# Delete session (logout)
DEL session:123
```

---
layout: default
---

# Real-time Sales Analytics

```js {all|1-4|6-8|10-12|all}
// Increment sales counter
MULTI
INCR sales:total
ZADD sales:hourly 1623456789 1

// Get sales statistics
GET sales:total
ZRANGE sales:hourly -24 -1

// Store in PostgreSQL
PUBLISH analytics:update "{
  total: 1000,
  hourly: [...]
}"
```

---
layout: default
---

# Concert Ticket Booking

```js {monaco}
// Atomic ticket reservation
const ticketReservation = async (showId, userId) => {
  const result = await redis.multi()
    .decr(`tickets:${showId}:available`)
    .zadd(`tickets:${showId}:reserved`, Date.now(), userId)
    .exec();
  
  if (result[0] < 0) {
    // Sold out - rollback
    await redis.multi()
      .incr(`tickets:${showId}:available`)
      .zrem(`tickets:${showId}:reserved`, userId)
      .exec();
    return false;
  }
  return true;
};
```

---
layout: end
---

# Thank You!

Khadev ขาเดฟ

https://www.facebook.com/khadev


