# Quick Start - Augment Conversation Data Manager

## One-Command Setup

```bash
./setup-conversation-manager.sh
```

This will:
1. ✅ Check prerequisites (Docker, Python 3)
2. ✅ Start PostgreSQL and Redis containers
3. ✅ Wait for services to be ready
4. ✅ Install Python dependencies
5. ✅ Initialize database schema

## Test the Installation

```bash
python3 test-conversation-manager.py
```

Expected output:
```
🧪 Testing Augment Conversation Data Manager
============================================================
✅ Connected to PostgreSQL: localhost:5432/augment_conversations
✅ Connected to Redis: localhost:6379/0

🧪 Test 1: Import Conversation
✅ Test 1 passed: Import successful

🧪 Test 2: Duplicate Detection
✅ Test 2 passed: Duplicate detection works

🧪 Test 3: Search
✅ Test 3 passed: Found 1 conversations

🧪 Test 4: Get Conversation
✅ Test 4 passed: Retrieved full conversation

🧪 Test 5: Statistics
✅ Test 5 passed: Statistics retrieved
   Conversations: 1
   Messages: 2
   Redis keys: 3

🧪 Test 6: Caching
✅ Test 6 passed: Caching works

============================================================
✅ All tests passed!
============================================================
```

## Import Your First Conversation

```bash
python3 conversation_manager.py import "Accessibility Upgrade for Login Controls_2026-01-05T22-59-10.json"
```

## Common Commands

### Search
```bash
python3 conversation_manager.py search "Accessibility"
```

### Get Full Conversation
```bash
python3 conversation_manager.py get "5f9624e7-46a7-40f2-afad-8615d839c692" --output conversation.json
```

### View Statistics
```bash
python3 conversation_manager.py stats
```

### Clear Cache
```bash
python3 conversation_manager.py clear-cache
```

## Verify Services Are Running

```bash
docker-compose -f docker-compose-conversation-manager.yml ps
```

Expected output:
```
NAME                IMAGE                COMMAND                  STATUS
augment-postgres    postgres:15-alpine   "docker-entrypoint.s…"   Up (healthy)
augment-redis       redis:7-alpine       "docker-entrypoint.s…"   Up (healthy)
```

## Stop Services

```bash
docker-compose -f docker-compose-conversation-manager.yml down
```

## Reset Everything (Delete All Data)

```bash
docker-compose -f docker-compose-conversation-manager.yml down -v
./setup-conversation-manager.sh
```

## Troubleshooting

### "Connection refused" errors

Services might not be ready yet. Wait 10 seconds and try again:
```bash
sleep 10
python3 conversation_manager.py stats
```

### Check PostgreSQL logs
```bash
docker-compose -f docker-compose-conversation-manager.yml logs postgres
```

### Check Redis logs
```bash
docker-compose -f docker-compose-conversation-manager.yml logs redis
```

### Connect to PostgreSQL directly
```bash
docker exec -it augment-postgres psql -U postgres -d augment_conversations
```

Example queries:
```sql
-- Count conversations
SELECT COUNT(*) FROM conversations;

-- List all conversations
SELECT conversation_id, name, total_messages FROM conversations;

-- Find duplicates
SELECT * FROM duplicate_messages;

-- Exit
\q
```

### Connect to Redis directly
```bash
docker exec -it augment-redis redis-cli
```

Example commands:
```
# List all keys
KEYS augment:*

# Get a cached conversation
GET augment:conversation:5f9624e7-46a7-40f2-afad-8615d839c692

# Clear all cache
FLUSHDB

# Exit
exit
```

## Full Documentation

See [CONVERSATION_MANAGER_README.md](CONVERSATION_MANAGER_README.md) for complete documentation.

