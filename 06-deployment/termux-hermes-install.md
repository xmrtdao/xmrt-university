# Termux Hermes Install

Complete guide to installing Hermes agent on Android.

## Prerequisites

- Android device with Termux installed
- ~500MB free storage
- Stable internet connection

## Installation

### 1. Install Termux

Download from F-Droid (recommended) or Google Play:
- **F-Droid:** https://f-droid.org/en/packages/com.termux/
- **Google Play:** Search "Termux"

### 2. Update Packages

```bash
pkg update && pkg upgrade -y
```

### 3. Install Dependencies

```bash
pkg install nodejs-lts python git curl wget -y
```

### 4. Clone Hermes

```bash
git clone https://github.com/xmrtdao/mobilemonero.git
cd mobilemonero
```

### 5. Install Python Dependencies

```bash
pip install requests python-dotenv
```

### 6. Configure Environment

```bash
cp .env.example .env
nano .env
```

**Required variables:**
```bash
SUPABASE_URL=https://vawouugtzwmejxqkeqqj.supabase.co
SUPABASE_ANON_KEY=your-anon-key
GITHUB_TOKEN=your-github-token
MUAPI_API_KEY=your-muapi-key
```

### 7. Start Services

```bash
# Start mesh router
node hermes-mesh.mjs --port=9001 &

# Start heartbeat daemon
python mesh-heartbeat.py &

# Verify services
curl http://localhost:9001/status
```

## Configuration

### Mesh Settings

Edit `hermes-mesh.mjs`:

```javascript
const config = {
  port: 9001,
  relayUrl: 'https://relay.mobilemonero.com',
  heartbeatInterval: 30000, // 30 seconds
  topics: [
    'agent-heartbeat',
    'agent-tasks',
    'agent-discovery',
    'fleet-broadcast'
  ]
};
```

### Heartbeat Settings

Edit `mesh-heartbeat.py`:

```python
CONFIG = {
    'interval': 30,  # seconds
    'relay_url': 'https://relay.mobilemonero.com',
    'agent_name': 'Hermes'
}
```

## Running Hermes

### Start All Services

```bash
# Create startup script
cat > start-hermes.sh << 'EOF'
#!/data/data/com.termux/files/usr/bin/bash
cd /data/data/com.termux/files/home/mobilemonero

# Start mesh router
node hermes-mesh.mjs --port=9001 &

# Start heartbeat
python mesh-heartbeat.py &

echo "Hermes started!"
EOF

chmod +x start-hermes.sh
./start-hermes.sh
```

### Check Status

```bash
# Check running processes
ps aux | grep -E "node|python"

# Check mesh status
curl http://localhost:9001/status

# Check relay connection
curl https://relay.mobilemonero.com/mesh/status
```

### Stop Services

```bash
# Stop mesh router
pkill -f hermes-mesh.mjs

# Stop heartbeat
pkill -f mesh-heartbeat.py
```

## Auto-Start on Boot

Create Termux boot script:

```bash
mkdir -p ~/.termux/boot
cat > ~/.termux/boot/hermes.sh << 'EOF'
#!/data/data/com.termux/files/usr/bin/bash
cd /data/data/com.termux/files/home/mobilemonero
node hermes-mesh.mjs --port=9001 &
python mesh-heartbeat.py &
EOF

chmod +x ~/.termux/boot/hermes.sh
```

## Troubleshooting

### Port Already in Use

```bash
# Find process using port
netstat -tlnp | grep 9001

# Kill it
kill <PID>

# Or use different port
node hermes-mesh.mjs --port=9002
```

### npm Install Fails

```bash
# Clear npm cache
npm cache clean --force

# Try again
npm install

# Or use yarn
pkg install yarn -y
yarn install
```

### Python Import Errors

```bash
# Reinstall dependencies
pip uninstall requests python-dotenv -y
pip install requests python-dotenv
```

### Mesh Connection Issues

```bash
# Check internet connection
ping -c 4 relay.mobilemonero.com

# Check firewall
pkg install net-tools -y
netstat -tlnp

# Restart services
pkill -f hermes-mesh.mjs
pkill -f mesh-heartbeat.py
./start-hermes.sh
```

### Storage Space Low

```bash
# Check disk usage
df -h

# Clean npm cache
npm cache clean --force

# Remove old logs
rm -rf ~/.hermes/logs/*.log

# Uninstall unused packages
pkg uninstall <package-name>
```

## Performance Tips

1. **Use background processes:** Run services with `&`
2. **Limit log output:** Redirect to files
3. **Monitor battery:** Hermes uses ~5% battery/hour
4. **Keep screen on:** Use Termux:Boot for persistence
5. **Use WiFi:** Faster than mobile data for large transfers

## Security

### Keep Termux Updated

```bash
pkg update && pkg upgrade -y
```

### Use Strong Passwords

```bash
# Set Termux password
passwd
```

### Limit Permissions

Don't run as root unless necessary:

```bash
# Check if running as root
whoami

# Should return: u0_aXXX (not root)
```

### Secure Environment Variables

```bash
# Set proper permissions
chmod 600 .env

# Don't commit .env to git
echo ".env" >> .gitignore
```

## Resources

- [Node Architecture](node-architecture.md)
- [Relay Endpoints](../01-ecosystem-apis/relay-endpoints.md)
- [Mesh Networking](../01-ecosystem-apis/mesh-networking.md)
