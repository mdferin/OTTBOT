# OTT Telegram Bot

Telegram bot for managing OTT (Online Television Tuner) URLs with mandatory subscription to a Telegram channel.

## 🚀 Main Features

### 1. Mandatory Subscription System
- Forces users to subscribe to Telegram channel @onmyid before using the bot
- Direct verification using Telegram API
- Buttons for channel subscription and subscription verification

### 2. OTT Account Management
- `/start` - Start the bot and access main menu
- `/info` - View your OTT URL information
- `/resetdevice` - Reset registered devices

### 3. Redeem System
- "🎁 Redeem URL OTT" button to get a new URL
- Queue system to manage redeem requests
- Free trial period (10 days)

### 4. Information and Pricing
- "💵 OTT Prices" button to view price list
- "ℹ️ OTT URL Info" button to view account information

### 5. Renewal System
- "🔄 Renew URL" button for users whose URL has expired

### 6. Automatic Notifications
- Notifications before and after URL expiration (7, 3, 1 days)
- Scheduled system for sending notifications

## 🔧 Admin Functions

### 1. Statistics and Monitoring
- `/stats` - View user statistics
- `/queue` - View redeem queue size
- `/ping` - Check system health

### 2. User Management
- `/notify <user_id|all> <days>` - Send manual notifications
- `/delete <user_id>` - Delete user
- `/resetnotify <user_id>` - Reset user notifications

### 3. Broadcast System
- `/broadcast` - Send to ALL users (text only)
- `/broadcastto <user_id>` - Send to ONE user (text only)
- `/broadcastlist <user_id1> <user_id2> ...` - Send to a LIST of users (text only)

## ⚙️ Configuration

### Main Settings
- `BOT_TOKEN` - Telegram Bot Token
- `SAVE_PATH` - Location for storing user files
- `LOG_FILE` - Redeem log location
- `BROADCAST_LOG_FILE` - Broadcast log location
- `NOTIFICATION_DAYS` - Days to send notifications before expiration
- `ADMIN_ID` - Telegram admin ID
- `CHANNEL_USERNAME` - Username of the mandatory Telegram channel

### File System
- Per-UUID files stored in database directory
- System logs using JSON format
- File lock system to prevent conflicts

## 🛡️ Security

### Subscription Verification
- Users must subscribe to channel @onmyid to use the bot
- Direct verification using Telegram API
- All bot functions check user subscription status

### Rate Limiting
- Retry system to avoid Telegram rate limits
- Scheduled notifications to reduce system load

## 📊 Queue System

### Redeem Queue
- Queue to manage concurrent redeem requests
- System worker to process requests sequentially
- Queue status viewable by admin

## 📝 File Formats

### redeem_log.json
```json
{
  "user_id": {
    "nama": "User Name",
    "uuid": "Unique ID",
    "redeem_time": timestamp,
    "expired_time": timestamp,
    "notified_days": [7, 3, 1]
  }
}
```

### Per-UUID File (database/uuid.json)
```json
{
  "expired_time": timestamp,
  "expired_desc": "Expiration Date",
  "limit_device": "1",
  "playlist_type": "all",
  "created_by": "DilarangBodoh"
}
```

## 🤖 Command Handler

### Regular Users
- `/start` - Start bot
- `/info` - View information
- `/resetdevice` - Reset device
- `/help` - Help

### Admin
- `/stats` - User statistics
- `/notify` - Manual notifications
- `/delete` - Delete user
- `/resetnotify` - Reset notifications
- `/ping` - Check system
- `/queue` - View queue
- `/broadcast` - Broadcast to all
- `/broadcastto` - Broadcast to one
- `/broadcastlist` - Broadcast to list

## 🔄 Callback Handler

### Inline Buttons
- `redeem` - Redeem OTT URL
- `info` - OTT URL Info
- `harga` - OTT Prices
- `resetdevice` - Reset Device
- `renew` - Renew URL
- `check_subscription` - Check subscription

## 📈 Notification System

### Automatic
- Runs every 6 hours
- Checks users who will expire
- Sends notifications 7, 3, and 1 days before expiration
- Sends notifications when already expired

### Manual
- Admin can send notifications at any time
- Can be sent to all users or specific users
- Can specify days before/after expiration

## 🛠️ Technical Functions

### Utility Functions
- `load_redeem_log()` - Load redeem log
- `safe_write()` - Write file safely
- `save_redeem_log()` - Save redeem log
- `get_user_name()` - Get user name
- `short_id()` - Generate unique ID
- `is_expired()` - Check expiration status
- `safe_send()` - Send message with retry
- `alert_admin()` - Send alert to admin
- `load_broadcast_log()` - Load broadcast log
- `save_broadcast_log()` - Save broadcast log
- `generate_message_hash()` - Generate message hash
- `is_user_subscribed()` - Check user subscription

### Worker Functions
- `redeem_worker()` - Process redeem queue
- `notification_task()` - Scheduled notification task

### Broadcast Functions
- `broadcast()` - Broadcast to all users
- `broadcastto()` - Broadcast to one user
- `broadcastlist()` - Broadcast to list of users
- `handle_broadcast_confirmation()` - Handle broadcast confirmation

## 🔐 Data Security

### Rate Limit Protection
- Automatic retry for network errors
- Delays between sends to avoid rate limits
- Background task scheduling

### Error Management
- Detailed error logging
- Automatic alerts to admin for critical errors
- Exception handling for all main functions

## 📦 Dependencies

### Main Libraries
- `python-telegram-bot` - Telegram API
- `pytz` - Time zone management
- `asyncio` - Asynchronous programming
- `json` - JSON data format management
- `logging` - Logging system
- `os` - File system interaction
- `random` & `string` - Random ID generation

### Minimum Versions
- Python 3.7+
- python-telegram-bot 20.0+

## 🚀 Deployment

### System Requirements
- Linux/Windows server with Python 3.7+
- Internet access for Telegram API connection
- Sufficient storage for database files
- Sufficient RAM for asynchronous operations

### Deployment Steps
1. Install dependencies: `pip install python-telegram-bot`
2. Set configuration in code (bot token, paths, etc)
3. Run bot: `python bot.py`
4. Ensure database directory is writable

### Maintenance
- Monitor system logs for errors
- Regularly backup database files
- Update bot token if needed
- Monitor server resource usage
