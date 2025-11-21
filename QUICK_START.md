# OpenBullet Quick Start Guide

## What is OpenBullet?

OpenBullet is a web testing automation suite that allows you to:
- Automate HTTP requests to web applications
- Test credentials, scrape data, perform automated testing
- Use Selenium for browser automation
- Manage proxies and wordlists
- Create reusable automation workflows

## Quick Setup

### 1. Build the Project

```bash
# Using Visual Studio
# 1. Open OpenBullet.sln
# 2. Switch to Release mode
# 3. Build Solution (Ctrl+Shift+B)

# Or using MSBuild
msbuild OpenBullet.sln /p:Configuration=Release
```

### 2. Run the Application

**GUI Version:**
```bash
cd OpenBullet/bin/Release
./OpenBullet.exe
```

**CLI Version:**
```bash
cd OpenBulletCLI/bin/Release
./OpenBulletCLI.exe --help
```

## Your First Config

### Step 1: Create a Simple Config

Create a file `Configs/test.loli`:

```loli
[SETTINGS]
{
  "SuggestedBots": 1,
  "MaxCPM": 60,
  "MaxRetries": 3,
  "OnlySocks": false,
  "OnlySsl": false,
  "AllowedWordlist1": "Default",
  "AllowedWordlist2": "",
  "CustomInputs": []
}

[SCRIPT]
BLOCK:Request
  URL=https://httpbin.org/get
  Method=GET
ENDBLOCK

BLOCK:Keycheck
  Keys=SUCCESS|FAIL
  SuccessOn=SUCCESS
ENDBLOCK
```

### Step 2: Create a Wordlist

Create a file `Wordlists/test.txt`:
```
test1
test2
test3
```

### Step 3: Run via CLI

```bash
OpenBulletCLI.exe \
  --config "Configs/test.loli" \
  --wordlist "Wordlists/test.txt" \
  --wltype "Default" \
  --output "Hits/results.txt" \
  --bots 1 \
  --verbose
```

### Step 4: Run via GUI

1. Open OpenBullet.exe
2. Go to **Configs** section
3. Click **New Config** or import your `.loli` file
4. Go to **Runner** section
5. Select your config
6. Load a wordlist
7. Click **Start**

## Understanding Configs

### Config Structure

A config has two main parts:

1. **SETTINGS** (JSON): Configuration metadata
   - `SuggestedBots`: Default number of concurrent bots
   - `MaxCPM`: Maximum requests per minute
   - `MaxRetries`: Retry attempts on failure
   - `AllowedWordlist1/2`: Allowed wordlist types

2. **SCRIPT** (LoliScript): The automation workflow
   - Blocks define actions (Request, Parse, Keycheck, etc.)
   - Variables like `<USERNAME>`, `<PASSWORD>` are replaced with wordlist data

### Common Blocks

#### REQUEST Block
```loli
BLOCK:Request
  URL=https://example.com/api
  Method=POST
  PostData=username=<USERNAME>&password=<PASSWORD>
  Headers=Content-Type: application/json
ENDBLOCK
```

#### PARSE Block
```loli
BLOCK:Parse
  Source=SOURCE
  LeftString=<div class="result">
  RightString=</div>
  OutputVariable=RESULT
ENDBLOCK
```

#### KEYCHECK Block
```loli
BLOCK:Keycheck
  Keys=SUCCESS|Welcome|Logged in
  SuccessOn=SUCCESS
ENDBLOCK
```

#### FUNCTION Block
```loli
BLOCK:Function
  Function=MD5
  Input=<DATA>
  OutputVariable=HASH
ENDBLOCK
```

## Wordlist Formats

### Default Format
```
line1
line2
line3
```

### Credentials Format (username:password)
```
admin:password123
user:secret456
```

### Email Format
```
user@example.com
test@domain.com
```

## Proxy Setup

### Proxy File Format
```
127.0.0.1:8080
proxy.example.com:3128
user:pass@proxy.com:8080
```

### Using Proxies

**CLI:**
```bash
--proxies "proxies.txt" --ptype Http --useproxies
```

**GUI:**
1. Go to **Proxy Manager**
2. Click **Add Proxies**
3. Import your proxy file
4. In Runner, enable proxies

## Common Workflows

### 1. Login Testing

```loli
[SCRIPT]
BLOCK:Request
  URL=https://example.com/login
  Method=POST
  PostData=username=<USERNAME>&password=<PASSWORD>
ENDBLOCK

BLOCK:Keycheck
  Keys=Dashboard|Welcome|Success
  SuccessOn=Dashboard
ENDBLOCK
```

### 2. Data Scraping

```loli
[SCRIPT]
BLOCK:Request
  URL=https://example.com/page/<DATA>
  Method=GET
ENDBLOCK

BLOCK:Parse
  Source=SOURCE
  LeftString=<title>
  RightString=</title>
  OutputVariable=TITLE
ENDBLOCK
```

### 3. API Testing

```loli
[SCRIPT]
BLOCK:Request
  URL=https://api.example.com/users
  Method=GET
  Headers=Authorization: Bearer <TOKEN>
ENDBLOCK

BLOCK:Parse
  Source=SOURCE
  LeftString="id":
  RightString=,
  OutputVariable=USER_ID
ENDBLOCK
```

## Tips & Best Practices

1. **Start Small**: Test with 1-2 bots first
2. **Use Proxies**: Rotate proxies to avoid rate limits
3. **Monitor CPM**: Keep requests per minute reasonable
4. **Validate Responses**: Always use Keycheck blocks
5. **Save Progress**: Export hits regularly
6. **Test Configs**: Verify configs work before large runs

## Troubleshooting

### Config won't load
- Check syntax (all blocks must have ENDBLOCK)
- Verify JSON in SETTINGS section is valid

### No hits found
- Check Keycheck conditions match actual responses
- Enable verbose mode to see responses
- Verify wordlist format matches config expectations

### Proxy errors
- Test proxies individually
- Check proxy format (IP:PORT or user:pass@IP:PORT)
- Verify proxy type matches (Http/Socks4/Socks5)

## Next Steps

1. Read the full [USAGE_GUIDE.md](USAGE_GUIDE.md)
2. Explore example configs in the community
3. Learn LoliScript syntax
4. Experiment with different block types
5. Join the [forum](https://discourse.openbullet.dev/) for help

## Important Notes

⚠️ **Legal Warning**: Only use OpenBullet on sites you own or have explicit permission to test. Unauthorized testing is illegal.

⚠️ **End of Life**: OpenBullet 1 is no longer maintained. Consider migrating to [OpenBullet 2](https://github.com/openbullet/OpenBullet2).

