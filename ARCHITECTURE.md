# OpenBullet Architecture Overview

## Project Structure

```
openbullet/
├── OpenBullet/              # GUI Application (WPF)
│   ├── Views/               # UI Views (XAML)
│   ├── ViewModels/          # MVVM ViewModels
│   ├── Models/              # UI-specific models
│   └── MainWindow.xaml.cs   # Main entry point
│
├── OpenBulletCLI/           # Command-Line Interface
│   └── Program.cs           # CLI entry point
│
├── RuriLib/                 # Core Library
│   ├── Blocks/             # Block implementations
│   ├── Runner/             # Execution engine
│   ├── LoliScript/          # Script parser/interpreter
│   ├── Models/             # Data models
│   ├── Functions/          # Utility functions
│   └── IOManager.cs        # File I/O operations
│
└── PluginFramework/         # Plugin system
    └── Interfaces/          # Plugin contracts
```

## Core Components

### 1. RuriLib (Core Library)

**Purpose**: The heart of OpenBullet - handles all automation logic.

**Key Classes:**
- `RunnerViewModel`: Manages multi-threaded execution
- `RunnerBotViewModel`: Individual bot instance
- `LoliScript`: Script parser and executor
- `BlockBase`: Base class for all blocks
- `Config`: Configuration model
- `BotData`: Data container for each bot

**Execution Flow:**
```
RunnerViewModel.Start()
  ├── Creates N RunnerBotViewModel instances (bots)
  ├── Each bot runs in separate thread
  ├── Bot loads wordlist line → BotData
  ├── Bot executes LoliScript blocks sequentially
  ├── Blocks modify BotData (variables, cookies, etc.)
  └── Results stored (Hits, Fails, Custom, ToCheck)
```

### 2. Blocks System

**Block Types:**

#### HTTP Blocks
- `BlockRequest`: HTTP/HTTPS requests
- `BlockBypassCF`: Cloudflare bypass
- `BlockTCP`: TCP socket communication

#### Parsing Blocks
- `BlockParse`: Extract data from responses
- `BlockKeycheck`: Check for success/failure patterns

#### Automation Blocks
- `BlockFunction`: Execute C# functions
- `BlockLSCode`: Execute LoliScript code
- `BlockUtility`: Various utilities

#### Captcha Blocks
- `BlockCaptcha`: Image captcha solving
- `BlockRecaptcha`: reCAPTCHA solving
- `BlockSolveCaptcha`: Generic captcha solving
- `BlockReportCaptcha`: Report bad captcha solutions

#### Selenium Blocks
- `SBlockNavigate`: Navigate to URL
- `SBlockBrowserAction`: Browser actions
- `SBlockElementAction`: Element interactions
- `SBlockExecuteJS`: Execute JavaScript

**Block Execution:**
```csharp
// Each block implements:
public override BlockBase FromLS(string line)
public override string ToLS(bool indent = false)
public override void Process(BotData data)
```

### 3. LoliScript Parser

**Purpose**: Parses `.loli` config files into executable blocks.

**Parser Components:**
- `LoliScript`: Main script executor
- `BlockParser`: Identifies and parses blocks
- `LineParser`: Parses individual lines
- `Writer`: Converts blocks back to LoliScript

**Script Format:**
```loli
BLOCK:Request
  URL=https://example.com
  Method=GET
ENDBLOCK
```

**Execution:**
1. Script split into lines
2. Each line checked for block type
3. Block parsed and instantiated
4. Block executed with BotData
5. BotData updated with results

### 4. Runner System

**Multi-threading Architecture:**

```
Master Worker (RunnerViewModel)
  │
  ├── Bot 1 (RunnerBotViewModel) ──┐
  ├── Bot 2 (RunnerBotViewModel) ──┤
  ├── Bot 3 (RunnerBotViewModel) ──┼──> Execute Config
  ├── ...                          │
  └── Bot N (RunnerBotViewModel) ──┘
```

**Bot Lifecycle:**
1. Bot created and assigned thread
2. Bot loads next wordlist line
3. Bot creates BotData instance
4. Bot executes LoliScript blocks
5. Bot processes results (Hit/Fail/Custom/ToCheck)
6. Bot repeats until wordlist exhausted or stopped

**Synchronization:**
- Thread-safe collections for results
- Locking mechanisms for shared resources
- Progress tracking via atomic counters

### 5. Data Flow

**BotData Structure:**
```csharp
public class BotData
{
    public string Data { get; set; }           // Current wordlist line
    public string Source { get; set; }         // Last response
    public Dictionary<string, string> Variables // Custom variables
    public CookieDictionary Cookies            // HTTP cookies
    public CProxy Proxy                        // Current proxy
    // ... more properties
}
```

**Variable System:**
- `<DATA>`: Current wordlist line
- `<USERNAME>`, `<PASSWORD>`: Parsed from wordlist
- `<SOURCE>`: Last HTTP response
- Custom variables: `<VARNAME>`

**Variable Replacement:**
- Variables replaced before block execution
- Supports nested variables
- Supports function calls

### 6. Configuration System

**Config Structure:**
```csharp
public class Config
{
    public ConfigSettings Settings { get; set; }  // JSON settings
    public string Script { get; set; }            // LoliScript
}
```

**ConfigSettings (JSON):**
- `SuggestedBots`: Default bot count
- `MaxCPM`: Rate limiting
- `MaxRetries`: Retry attempts
- `AllowedWordlist1/2`: Wordlist type restrictions
- `CustomInputs`: User-defined inputs

**Config Loading:**
1. File read as string
2. Split into `[SETTINGS]` and `[SCRIPT]` sections
3. Settings deserialized from JSON
4. Script parsed into blocks

### 7. Proxy Management

**ProxyPool:**
- Manages proxy rotation
- Tracks proxy health
- Supports multiple proxy types (Http, Socks4, Socks5)

**Proxy Assignment:**
- Each bot gets proxy from pool
- Proxies rotated per request or per bot
- Dead proxies removed from pool

### 8. Wordlist System

**Wordlist Types:**
- Defined in `Environment.ini`
- Regex validation
- Custom separators
- Data slicing (USERNAME, PASSWORD, etc.)

**Wordlist Processing:**
1. File loaded into memory
2. Lines validated against wordlist type regex
3. Lines split by separator
4. Slices assigned to variables
5. Lines distributed to bots

### 9. GUI Application (OpenBullet)

**Architecture**: MVVM (Model-View-ViewModel)

**Main Components:**
- `MainWindow`: Main application window
- `RunnerManager`: Manages multiple runners
- `ConfigManager`: Config creation/editing
- `Stacker`: Visual config editor
- `WordlistManager`: Wordlist management
- `ProxyManager`: Proxy management

**Views:**
- XAML-based UI
- Data binding to ViewModels
- Real-time updates via INotifyPropertyChanged

### 10. CLI Application (OpenBulletCLI)

**Purpose**: Headless execution for automation/scripts

**Features:**
- Command-line argument parsing
- Minimal UI (console output)
- Same execution engine as GUI
- Progress tracking via console title

**Limitations:**
- No visual config editor
- Limited settings management
- Basic logging only

## Execution Pipeline

```
1. User loads Config + Wordlist
   │
2. RunnerViewModel initialized
   │
3. Config parsed → List<BlockBase>
   │
4. Wordlist loaded → DataPool
   │
5. N bots created (threads)
   │
6. Each bot loop:
   │   ├── Get next wordlist line
   │   ├── Create BotData
   │   ├── Execute blocks sequentially
   │   │   ├── Block.Process(BotData)
   │   │   ├── Variables replaced
   │   │   ├── HTTP request made (if Request block)
   │   │   ├── Response parsed (if Parse block)
   │   │   └── Results checked (if Keycheck block)
   │   ├── Process results
   │   └── Repeat
   │
7. Results collected:
   │   ├── Hits → HitsDB
   │   ├── Fails → Logged
   │   ├── Custom → Custom list
   │   └── ToCheck → Manual review
   │
8. Statistics updated:
   │   ├── CPM calculated
   │   ├── Progress percentage
   │   └── Proxy health
```

## Key Design Patterns

1. **MVVM**: GUI separation of concerns
2. **Factory Pattern**: Block creation from strings
3. **Strategy Pattern**: Different block types
4. **Observer Pattern**: Event-driven updates
5. **Singleton Pattern**: Global settings/logger

## Thread Safety

**Synchronization Points:**
- `FileLocker`: File I/O locking
- `randomLocker`: Random number generation
- Collections: Thread-safe collections used
- Progress counters: Atomic operations

## Extension Points

1. **Plugins**: Custom blocks via PluginFramework
2. **Functions**: Custom C# functions
3. **Scripts**: JavaScript/IronPython execution
4. **Wordlist Types**: Custom regex patterns
5. **Export Formats**: Custom hit export formats

## Dependencies

**Key Libraries:**
- `Extreme.Net`: HTTP client
- `Selenium.WebDriver`: Browser automation
- `Newtonsoft.Json`: JSON serialization
- `LiteDB`: Database for hits
- `CaptchaSharp`: Captcha solving
- `IronPython`: Python scripting
- `Jint`: JavaScript execution
- `AngleSharp`: HTML parsing

## Performance Considerations

1. **Thread Pool**: Reuses threads for bots
2. **Memory Management**: Streams for large wordlists
3. **Proxy Rotation**: Efficient proxy selection
4. **Rate Limiting**: CPM throttling
5. **Batch Operations**: Bulk file operations

## Security Considerations

1. **Script Execution**: Sandboxed JavaScript/Python
2. **File Access**: Restricted to app directories
3. **Network**: Proxy support for anonymity
4. **Data**: Sensitive data in memory only

## Future Improvements (OpenBullet 2)

- Better plugin system
- Improved performance
- More block types
- Better error handling
- Modern UI framework
- Cross-platform support

