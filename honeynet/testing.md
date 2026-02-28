# OpenCanary Honeynet Testing Guide

This guide provides instructions on how to test each of the configured OpenCanary honeypot services and verify that they are correctly logging interaction attempts.

## Running the Honeynet

Before testing, ensure your honeynet is up and running. In your `honeynet` directory, run:

```powershell
docker compose up -d
```

Verify the containers are running:
```powershell
docker compose ps
```

## Testing Services

### 1. SSH Honeypot (Port 22)
To test the SSH honeypot, attempt to log in using a fake username and password.

**From Linux/macOS or WSL:**
```bash
ssh root@localhost
# Enter any fake password when prompted
```

**Verification:** Check the SSH honeypot logs for the login attempt.
```powershell
Get-Content .\logs\ssh\opencanary.log -Tail 10
# Or using bash: tail -n 10 ./logs/ssh/opencanary.log
```
You should see a JSON log entry with your failed login attempt details, including the `node_id` of `honeynet-ssh-01`.

### 2. HTTP/Web Honeypot (Port 80)
To test the web honeypot, simply make an HTTP request to the server.

**From Windows PowerShell:**
```powershell
Invoke-WebRequest -Uri http://localhost
```

**From Linux/macOS or WSL:**
```bash
curl http://localhost
```

**Verification:** Check the Web honeypot logs.
```powershell
Get-Content .\logs\web\opencanary.log -Tail 10
```
Look for a GET request logged by `honeynet-web-01`.

### 3. SMB Honeypot (Port 445)
To test the SMB Windows File Sharing honeypot:

**From Windows:**
1. Open File Explorer.
2. Type `\\localhost` or `\\127.0.0.1` in the address bar.
3. If prompted for credentials, enter anything.

**From Linux/macOS or WSL (if smbclient is installed):**
```bash
smbclient -L \\\\127.0.0.1 -U fakeuser
# Enter any password
```

**Verification:** Check the SMB honeypot logs.
```powershell
Get-Content .\logs\smb\opencanary.log -Tail 10
```
Look for connection attempts logged by `honeynet-smb-01`.

### 4. MySQL Honeypot (Port 3306)
To test the database honeypot, try to connect as the root user.

**From any system with a MySQL client:**
```bash
mysql -h 127.0.0.1 -P 3306 -u root -p
# Enter any fake password
```
*(If you do not have the mysql client installed locally, you can use telnet or netcat to connect to port 3306 just to trigger the connection log: `nc localhost 3306` or ping port via powershell `Test-NetConnection -ComputerName localhost -Port 3306`)*

**Verification:** Check the MySQL honeypot logs.
```powershell
Get-Content .\logs\mysql\opencanary.log -Tail 10
```
Look for authentication failure logs for `honeynet-mysql-01`.

## Stopping the Honeynet

When you are finished testing, you can shut down the honeynet gracefully:

```powershell
docker compose down
```
