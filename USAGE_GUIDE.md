# OpenBullet Usage Guide

## Overview

OpenBullet is a web testing suite written in C# (.NET Framework 4.7.2) that allows you to:
- Perform automated HTTP requests to web applications
- Scrape and parse data from websites
- Perform automated penetration testing
- Run Selenium-based browser automation
- Test credentials and perform other web automation tasks

**⚠️ IMPORTANT:** OpenBullet 1 has reached end-of-life. Consider migrating to [OpenBullet 2](https://github.com/openbullet/OpenBullet2) for continued support.

## Project Structure

```
openbullet/
├── OpenBullet/          # GUI Application (WPF)
├── OpenBulletCLI/       # Command-Line Interface
├── RuriLib/             # Core Library (blocks, runner, etc.)
├── PluginFramework/      # Plugin System
└── OpenBullet.sln       # Visual Studio Solution
```

## Building the Project

### Prerequisites
- .NET Framework 4.7.2 Developer Pack
- Visual Studio (or MSBuild)
- NuGet package manager

### Build Steps
1. Clone the repository
2. Open `OpenBullet.sln` in Visual Studio
3. Switch to **Release** mode for production builds
4. Build the solution (NuGet packages will be restored automatically)
5. Executables will be in:
   - `OpenBullet/bin/Release/` - GUI application
   - `OpenBulletCLI/bin/Release/` - CLI application

## Using the GUI Application

### First Run Setup
When you first run OpenBullet, it will create the following directories:
- `Captchas/` - Captcha solving configurations
- `ChromeExtensions/` - Chrome extension files
- `Configs/` - Configuration files (.loli format)
- `DB/` - Database files
- `Hits/` - Successful hits/results
- `Plugins/` - Custom plugins
- `Screenshots/` - Screenshots from Selenium
- `Settings/` - Application settings
- `Sounds/` - Sound files
- `Wordlists/` - Wordlist files

### Main Features

#### 1. Configs Section
- Create/edit configurations using the visual "Stacker" editor
- Configs define the workflow: HTTP requests, parsing, keychecks, etc.
- Configs are saved in `.loli` format (LoliScript)

#### 2. Runner Manager
- Load a config and wordlist
- Set number of concurrent bots
- Configure proxy settings
- Start/stop the runner
- Monitor progress, hits, and statistics

#### 3. Wordlist Manager
- Import wordlists (text files)
- Define wordlist types (Default, Email, Credentials, etc.)
- Configure separators and data slices

#### 4. Proxy Manager
- Import proxy lists
- Test proxy connectivity
- Manage proxy pools

#### 5. Hits Database
- View successful hits
- Export results
- Filter and search hits

## Using the CLI Application

The CLI is a proof-of-concept implementation with limited features compared to the GUI.

### Basic Usage

```bash
OpenBulletCLI.exe \
  --config "config.loli" \
  --wordlist "wordlist.txt" \
  --wltype "Default" \
  --output "hits.txt" \
  --proxies "proxies.txt" \
  --ptype Http \
  --useproxies \
  --bots 10 \
  --skip 1 \
  --verbose
```

### CLI Options

| Option | Required | Description |
|--------|----------|-------------|
| `-c, --config` | Yes | Configuration file (.loli) |
| `-w, --wordlist` | Yes | Wordlist file |
| `--wltype` | Yes | Wordlist type (see Environment.ini) |
| `-o, --output` | No | Output file for hits |
| `-p, --proxies` | No | Proxy file |
| `--ptype` | No | Proxy type (Http, Socks4, Socks5) |
| `--useproxies` | No | Enable/disable proxies |
| `-b, --bots` | No | Number of concurrent bots (0 = use config default) |
| `-s, --skip` | No | Lines to skip in wordlist (default: 1) |
| `-v, --verbose` | No | Print detailed bot behavior |

### Example CLI Command

```bash
OpenBulletCLI.exe \
  --config "Configs/myconfig.loli" \
  --wordlist "Wordlists/rockyou.txt" \
  --wltype "Credentials" \
  --output "Hits/results.txt" \
  --proxies "proxies.txt" \
  --ptype Http \
  --useproxies \
  --bots 5 \
  --verbose
```

## Configuration Files (.loli Format)

Configs use LoliScript, a domain-specific language for defining automation workflows.

### Basic Config Structure

```loli
# Config metadata
[CONFIG]
Name=My Test Config
Category=Testing
Author=YourName

# Settings
[SETTINGS]
SuggestedBots=10
MaxCPM=60
MaxRetries=3
OnlySocks=false
OnlySsl=false

# Blocks (workflow)
[BLOCKS]
BLOCK:Request
  URL=https://example.com/login
  Method=POST
  PostData=username=<USERNAME>&password=<PASSWORD>
  Headers=Content-Type: application/x-www-form-urlencoded
ENDBLOCK

BLOCK:Keycheck
  Keys=SUCCESS|FAIL
  SuccessOn=SUCCESS
ENDBLOCK
```

### Available Block Types

1. **Request** - HTTP/HTTPS requests
2. **Parse** - Extract data from responses
3. **Keycheck** - Check for success/failure indicators
4. **Function** - Execute C# functions
5. **LSCode** - Execute LoliScript code
6. **Captcha** - Solve captchas
7. **Recaptcha** - Solve reCAPTCHA
8. **BypassCF** - Bypass Cloudflare protection
9. **TCP** - TCP socket communication
10. **Utility** - Various utility functions
11. **Selenium Blocks** - Browser automation (Navigate, ElementAction, ExecuteJS, etc.)

## Wordlist Types

Wordlists are defined in `Settings/Environment.ini`:

- **Default**: Any format (`^.*$`)
- **Email**: Email addresses (`^[^@]+@[^\.]+\..+$`)
- **Credentials**: Username:Password format (`^.*:.*$`)
- **Numeric**: Numbers only (`^[0-9]*$`)
- **URLs**: URL format

You can define custom wordlist types with regex patterns.

## Environment Settings

The `Settings/Environment.ini` file defines:
- Wordlist types and their regex patterns
- Custom keychains (for hit categorization)
- Export formats

## RuriLib API

RuriLib is the core library that powers OpenBullet. It provides:
- Block execution engine
- HTTP client (Extreme.Net)
- Selenium integration
- Captcha solving (via CaptchaSharp)
- Proxy management
- Data parsing and manipulation

### Key Components

- **Runner**: Executes configs with wordlists
- **Blocks**: Individual workflow steps
- **BotData**: Contains data for each bot instance
- **ProxyPool**: Manages proxy rotation

## Plugin System

OpenBullet supports plugins for extending functionality:
- Place `.dll` files in the `Plugins/` directory
- Plugins can add custom blocks
- See [sample plugin](https://github.com/openbullet/openbullet-plugin) for examples

## Common Use Cases

### 1. Credential Testing
- Load a credentials wordlist (username:password format)
- Create a config that POSTs to login endpoint
- Use Keycheck to identify successful logins
- Export hits to database

### 2. Data Scraping
- Create a config to navigate pages
- Use Parse blocks to extract data
- Save results to hits database
- Export in desired format

### 3. API Testing
- Create configs for API endpoints
- Test different parameters
- Parse JSON responses
- Validate responses

### 4. Browser Automation
- Use Selenium blocks for complex interactions
- Handle JavaScript-heavy sites
- Take screenshots
- Execute custom JavaScript

## Best Practices

1. **Start Small**: Test configs with small wordlists first
2. **Use Proxies**: Rotate proxies to avoid rate limiting
3. **Monitor CPM**: Control requests per minute
4. **Handle Errors**: Configure retries and error handling
5. **Validate Data**: Use Keycheck blocks to verify responses
6. **Export Regularly**: Save hits to avoid data loss

## Troubleshooting

### Common Issues

1. **Config won't load**: Check syntax, ensure all blocks are properly closed
2. **No hits**: Verify Keycheck conditions match actual responses
3. **Proxy errors**: Test proxies individually, check proxy format
4. **Rate limiting**: Reduce bots/CPM, use more proxies
5. **Selenium errors**: Ensure ChromeDriver/FirefoxDriver is in PATH

## Documentation

- Official Documentation: https://openbullet.github.io/ob1
- Forum: https://discourse.openbullet.dev/
- RuriLib API: See RuriLib.xml documentation file

## Legal Notice

**⚠️ WARNING**: Performing (D)DoS attacks or credential stuffing on sites you do not own (or lack permission to test) is **illegal**. The developer is not responsible for improper use of this software.

Only use OpenBullet on:
- Sites you own
- Sites you have explicit written permission to test
- Your own applications for testing purposes

## Migration to OpenBullet 2

OpenBullet 1 is end-of-life. Consider migrating to OpenBullet 2 for:
- Continued support and updates
- More features and improvements
- Better performance
- Active development

## Support

- GitHub Issues: For bug reports
- Forum: For questions and community support
- Email: ruri [at] openbullet [dot] dev (not checked frequently)

