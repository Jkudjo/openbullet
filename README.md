# OpenBullet 2

![Logo](https://i.imgur.com/sToEkJC.png)

OpenBullet 2 is a cross-platform automation suite powered by .NET 8. It allows to perform requests towards a target webapp and offers a lot of tools to work with the results. This software can be used for **scraping** and **parsing data**, automated **penetration testing** and much more.

## ⚠️ Disclaimer

Performing (D)DoS or credential stuffing attacks on sites you do not have permission to test is **illegal!** The developer will not be held responsible for improper use of this software.

## 🚀 What's New in OpenBullet 2

OpenBullet 2 is a complete rewrite and upgrade from OpenBullet 1, featuring:

- **Cross-platform**: Runs on Windows, Linux, and macOS (.NET 8)
- **Modern Architecture**: Async/await patterns, better performance
- **Web UI**: Browser-based interface accessible from anywhere
- **Native Client**: Windows desktop application (optional)
- **REST API**: Programmatic access to all features
- **Guests System**: Multi-user support with permissions
- **Remote Configs**: Cloud-based configuration management
- **Better Plugins**: Enhanced plugin system
- **Docker Support**: Containerized deployment
- **Improved Performance**: Better multithreading and resource management

## 📚 Documentation

For detailed information on how to get started with **OpenBullet 2**, check out the [documentation](https://docs.openbullet.dev).

[![Docs](https://img.shields.io/badge/Docs-Read_the_Docs-2292A4.svg)](https://docs.openbullet.dev)

## 🌐 Community

Join our community to discuss and share your experiences with **OpenBullet 2**.

[![Forum](https://img.shields.io/badge/Forum-Join_the_community-2292A4.svg)](https://discourse.openbullet.dev/)

## 📦 Quick Start

### Prerequisites

- .NET 8 SDK ([Download](https://dotnet.microsoft.com/download/dotnet/8.0))
- Node.js 18+ (for web client, if building from source)

### Building from Source

```bash
# Clone the repository
git clone https://github.com/Jkudjo/OpenBullet2.git
cd OpenBullet2

# Restore dependencies
dotnet restore

# Build the solution
dotnet build OpenBullet2.sln --configuration Release

# Run the web server
cd OpenBullet2.Web
dotnet run
```

The web interface will be available at `http://localhost:5000` (or the port shown in the console).

### Docker Deployment

```bash
# Build and run with Docker Compose
docker-compose up -d

# Or build manually
docker build -t openbullet2 .
docker run -p 5000:5000 openbullet2
```

## 📸 Screenshots

**Web Client**

![Web Client](https://github.com/openbullet/OpenBullet2-Private/assets/48930622/4c009929-9254-4180-9c37-0b3a53efdbd3)

**Native Client** (Windows only)

![Native Client](https://user-images.githubusercontent.com/48930622/151500974-5cb7a9fd-766b-44ab-b32e-f7d623c0e7dd.png)

## 🏗️ Project Structure

```
OpenBullet2/
├── RuriLib/                    # Core automation library
├── RuriLib.Http/              # HTTP client functionality
├── RuriLib.Proxies/            # Proxy management
├── RuriLib.Parallelization/    # Parallel execution engine
├── OpenBullet2.Core/           # Core application logic
├── OpenBullet2.Web/            # Web server (Blazor)
├── OpenBullet2.Console/        # CLI application
├── OpenBullet2.Native/         # Windows native client
└── openbullet2-web-client/     # Web client frontend
```

## 🔄 Migration from OpenBullet 1

OpenBullet 2 can import `.loli` configuration files from OpenBullet 1. Simply import your existing configs - they will be automatically converted to the new `.opk` format.

**Note**: Some specific blocks or settings may not be fully compatible. Test your configs after migration.

## 🐛 Report Issues

Found a bug or have a feature request? Please help us improve by submitting an issue.

[![Open an issue](https://img.shields.io/badge/Bug-Report_a_bug-E74C3C.svg)](https://github.com/openbullet/OpenBullet2/issues/new?template=bug-report.yaml&title=%5BBug%5D%3A+) [![Request a feature](https://img.shields.io/badge/Feature-Request_a_feature-2292A4.svg)](https://github.com/openbullet/OpenBullet2/issues/new?template=feature_request.md&title=%5BREQUEST%5D)

## 📊 Stats

![Alt](https://repobeats.axiom.co/api/embed/c51394480e5e3da9259cae343f80dd1c53a8b263.svg "Repobeats analytics image")

## 📖 Additional Documentation

- **[USAGE_GUIDE.md](USAGE_GUIDE.md)**: Comprehensive usage guide (OpenBullet 1, still relevant)
- **[ARCHITECTURE.md](ARCHITECTURE.md)**: Technical architecture overview
- **[QUICK_START.md](QUICK_START.md)**: Quick start tutorial
- **[REPOSITORY_OVERVIEW.md](REPOSITORY_OVERVIEW.md)**: Repository overview

## 🔧 Legacy Code

OpenBullet 1 code has been moved to the `legacy/` directory for reference. It is no longer maintained but may be useful for understanding the evolution of the project.

## License

This software is licensed under the MIT License.

## Donate

If you like this software, consider making a donation to the developer. Thank you!
- BTC: **39yMkox6pP8tnSC7rZ5EM4nUUHgPbg1fKM**
- ETH: **0xc22116Bcf6c30977bEdFcc03C5B6aAe90B0fD179**
- BCH: **qq02mrtdp454g2zdu534ndpu7jgcr3tvavyzs60m3p**

## Credits

I want to thank all the community for their inputs that shaped OpenBullet into what it is now, and my gratitude goes especially towards my collaborators **demiurgo** and **meinname**.

## Contact

The best way to contact me is through the [official forum](https://discourse.openbullet.dev/u/Ruri). I'm not on discord / telegram.
If you need to contact me via mail for any reason you can send me a message here: `ruri [at] openbullet [dot] dev`. I don't check it very often so be patient please.

## About

OpenBullet reinvented - Modern, cross-platform web automation suite.

**Source**: Based on [OpenBullet2](https://github.com/Jkudjo/OpenBullet2) by Jkudjo
