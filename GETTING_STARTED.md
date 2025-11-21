# Getting Started with OpenBullet 2

## ✅ Upgrade Complete!

Your repository has been successfully upgraded to **OpenBullet 2** - a modern, cross-platform web automation suite.

## 🚀 Quick Start

### Prerequisites

1. **Install .NET 8 SDK**
   ```bash
   # Linux (Ubuntu/Debian)
   wget https://dotnet.microsoft.com/download/dotnet/scripts/v1/dotnet-install.sh
   chmod +x dotnet-install.sh
   ./dotnet-install.sh --channel 8.0

   # Or download from: https://dotnet.microsoft.com/download/dotnet/8.0
   ```

2. **Install Node.js 18+** (for web client, if building from source)
   ```bash
   # Linux
   curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
   sudo apt-get install -y nodejs
   ```

### Building the Project

```bash
# Navigate to repository
cd /home/hp/hacks/openbullet

# Restore NuGet packages
dotnet restore OpenBullet2.sln

# Build the solution
dotnet build OpenBullet2.sln --configuration Release

# Run the web server
cd OpenBullet2.Web
dotnet run
```

The web interface will be available at `http://localhost:5000` (check console output for exact port).

### Using Docker

```bash
# Build and run with Docker Compose
docker-compose up -d

# Access at http://localhost:8069
```

## 📁 Project Structure

```
openbullet/
├── RuriLib/                    # Core automation library
├── RuriLib.Http/               # HTTP client functionality  
├── RuriLib.Proxies/            # Proxy management
├── RuriLib.Parallelization/    # Parallel execution engine
├── OpenBullet2.Core/           # Core application logic
├── OpenBullet2.Web/            # Web server (Blazor)
├── OpenBullet2.Console/        # CLI application
├── OpenBullet2.Native/         # Windows native client
├── openbullet2-web-client/     # Web client frontend
└── legacy/                     # OpenBullet 1 (preserved)
```

## 🔄 Migrating from OpenBullet 1

### Import Configs
1. Open OpenBullet 2 Web UI
2. Go to **Configs** section
3. Click **Import** or **Add Config**
4. Select your `.loli` file
5. Config will be automatically converted to `.opk` format

### Import Wordlists
- Copy wordlist files to the new location
- Import through Wordlist Manager
- Formats are compatible (Default, Email, Credentials, etc.)

### Import Proxies
- Proxy formats are compatible
- Import through Proxy Manager
- Supports HTTP, SOCKS4, SOCKS5

See [MIGRATION_GUIDE.md](MIGRATION_GUIDE.md) for detailed instructions.

## 🎯 Key Features

### New in OpenBullet 2
- ✅ **Cross-platform**: Windows, Linux, macOS
- ✅ **Web UI**: Browser-based interface
- ✅ **REST API**: Programmatic access
- ✅ **Guests System**: Multi-user support
- ✅ **Remote Configs**: Cloud-based management
- ✅ **Docker Support**: Containerized deployment
- ✅ **Better Performance**: Async/await patterns
- ✅ **Modern Architecture**: .NET 8

### Core Capabilities
- HTTP/HTTPS request automation
- Data scraping and parsing
- Credential testing
- Browser automation (Selenium/Puppeteer)
- Proxy rotation
- Captcha solving
- Multi-threaded execution

## 📚 Documentation

- **[README.md](README.md)**: Main project documentation
- **[MIGRATION_GUIDE.md](MIGRATION_GUIDE.md)**: Migration from OB1
- **[UPGRADE_SUMMARY.md](UPGRADE_SUMMARY.md)**: Upgrade details
- **[USAGE_GUIDE.md](USAGE_GUIDE.md)**: Comprehensive usage guide
- **[ARCHITECTURE.md](ARCHITECTURE.md)**: Technical architecture

### External Resources
- **Official Docs**: https://docs.openbullet.dev
- **Forum**: https://discourse.openbullet.dev
- **GitHub**: https://github.com/Jkudjo/OpenBullet2

## 🛠️ Development

### Building Individual Projects

```bash
# Build core library
cd RuriLib
dotnet build

# Build web server
cd OpenBullet2.Web
dotnet build

# Run tests
cd RuriLib.Tests
dotnet test
```

### Creating Plugins

See plugin development documentation in the official docs.

### Using REST API

The web server exposes a REST API for programmatic access. Check API documentation for endpoints.

## 🐳 Docker Deployment

### Build Docker Image
```bash
docker build -t openbullet2 .
```

### Run Container
```bash
docker run -d \
  -p 8069:5000 \
  -v $(pwd)/UserData:/app/UserData \
  --name openbullet2 \
  openbullet2
```

### Docker Compose
```bash
docker-compose up -d
```

## ⚠️ Important Notes

1. **Legal**: Only use on sites you own or have explicit permission to test
2. **Security**: Change default passwords in production
3. **Backup**: Regularly backup your UserData folder
4. **Updates**: Keep OpenBullet 2 updated for latest features and security fixes

## 🆘 Troubleshooting

### Build Issues
- Ensure .NET 8 SDK is installed
- Run `dotnet restore` before building
- Check for missing dependencies

### Runtime Issues
- Check logs in `UserData/Logs/`
- Verify port availability (default: 5000)
- Check firewall settings

### Migration Issues
- Test imported configs with small wordlists first
- Check compatibility notes in MIGRATION_GUIDE.md
- Report issues on GitHub

## 🎉 Next Steps

1. **Build and Run**: Get OpenBullet 2 running
2. **Import Configs**: Migrate your OpenBullet 1 configs
3. **Explore Features**: Try new features like Guests, Remote Configs
4. **Customize**: Customize settings and UI if needed
5. **Deploy**: Set up production deployment (Docker recommended)

## 📞 Support

- **Documentation**: https://docs.openbullet.dev
- **Forum**: https://discourse.openbullet.dev
- **GitHub Issues**: Report bugs on GitHub
- **Email**: ruri [at] openbullet [dot] dev (not checked frequently)

---

**Welcome to OpenBullet 2!** 🚀

Enjoy the enhanced capabilities and modern architecture of OpenBullet 2.

