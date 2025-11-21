# Migration Guide: OpenBullet 1 → OpenBullet 2

This guide will help you migrate from OpenBullet 1 to OpenBullet 2.

## Overview

OpenBullet 2 is a complete rewrite with modern architecture, but maintains backward compatibility for most OpenBullet 1 features.

## Key Differences

### Platform
- **OB1**: .NET Framework 4.7.2 (Windows only)
- **OB2**: .NET 8 (Cross-platform: Windows, Linux, macOS)

### Configuration Format
- **OB1**: `.loli` files (LoliScript)
- **OB2**: `.opk` files (OpenBullet Package) - but can import `.loli`

### UI
- **OB1**: WPF desktop application
- **OB2**: Web-based UI (Blazor) + optional Windows native client

### Architecture
- **OB1**: Synchronous, thread-based
- **OB2**: Async/await, modern patterns

## Migration Steps

### 1. Install OpenBullet 2

Download and install OpenBullet 2 from the [official repository](https://github.com/Jkudjo/OpenBullet2) or build from source.

### 2. Import Configs

OpenBullet 2 can directly import `.loli` configuration files:

1. Open OpenBullet 2 Web UI
2. Go to Configs section
3. Click "Import" or "Add Config"
4. Select your `.loli` file
5. The config will be automatically converted to `.opk` format

### 3. Import Wordlists

Wordlist formats are compatible:
- Copy your wordlist files to the new location
- Import them through the Wordlist Manager
- Wordlist types (Default, Email, Credentials, etc.) are preserved

### 4. Import Proxies

Proxy formats are compatible:
- HTTP: `ip:port` or `user:pass@ip:port`
- SOCKS4/SOCKS5: Same format
- Import through Proxy Manager

### 5. Migrate Settings

Some settings may need manual configuration:
- Proxy settings
- Captcha solver API keys
- Selenium settings
- Custom wordlist types

### 6. Test Your Configs

After importing:
1. Test each config with a small wordlist
2. Verify all blocks work correctly
3. Check for any deprecated features
4. Update configs if needed

## Compatibility Notes

### Fully Compatible
- ✅ Request blocks
- ✅ Parse blocks
- ✅ Keycheck blocks
- ✅ Function blocks
- ✅ Most utility blocks
- ✅ Wordlist formats
- ✅ Proxy formats

### Partially Compatible
- ⚠️ Some Selenium blocks may need updates
- ⚠️ Custom plugins may need recompilation
- ⚠️ Some advanced LoliScript features

### Not Compatible
- ❌ Old plugin system (new plugin API)
- ❌ Some deprecated blocks
- ❌ Windows-specific features (if running on Linux/macOS)

## New Features in OB2

Take advantage of new features:

1. **Guests System**: Share configs with team members
2. **Remote Configs**: Host configs on remote servers
3. **REST API**: Automate via API calls
4. **Better Performance**: Improved multithreading
5. **Web UI**: Access from any device
6. **Docker**: Easy deployment

## Troubleshooting

### Config Won't Import
- Check file format (should be `.loli`)
- Verify config syntax is valid
- Check logs for specific errors

### Blocks Not Working
- Some blocks may have changed syntax
- Check documentation for updated block parameters
- Test blocks individually

### Performance Issues
- OB2 uses different threading model
- Adjust bot count if needed
- Check proxy settings

## Getting Help

- **Documentation**: https://docs.openbullet.dev
- **Forum**: https://discourse.openbullet.dev
- **GitHub Issues**: Report bugs on GitHub

## Legacy Code

OpenBullet 1 code is preserved in the `legacy/` directory for reference. You can still use it if needed, but it's no longer maintained.

## Conclusion

OpenBullet 2 offers significant improvements while maintaining compatibility with most OpenBullet 1 features. The migration process is straightforward, and most configs will work with minimal changes.

For detailed information, refer to the [official documentation](https://docs.openbullet.dev).

