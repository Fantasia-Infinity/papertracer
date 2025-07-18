# 🕷️ PaperTracer

Choose your language / 选择语言:

- [English Documentation](README_en.md)
- [中文文档](README_zh.md)

---

[![Python 3.7+](https://img.shields.io/badge/python-3.7+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Enhanced](https://img.shields.io/badge/Enhanced-v2.0-green.svg)](ENHANCED_FEATURES.md)

PaperTracer is a powerful Google Scholar citation crawler that recursively crawls academic paper citation networks and builds tree data structures. **Now with enhanced session management, intelligent rate limiting, and real-time performance monitoring!**

## ✨ Core Features

- 🔍 **Smart Parsing**: Automatically parses paper titles, authors, year, citation count, etc.
- 🌳 **Tree Structure**: Builds clear citation relationships trees, supporting multi-level recursion.
- 💾 **Data Export**: Supports JSON format export for further analysis.
- 🌐 **Interactive Visualization**: Generates interactive HTML pages supporting mouse hover, click access, etc.
- 📊 **Multiple Visualizations**: Supports both static charts and dynamic HTML visualization.
- 🛡️ **Anti-Crawler Protection**: Built-in random delay and User-Agent rotation mechanisms.
- ⚙️ **Flexible Configuration**: Customizable crawl depth, number of papers per level, etc.
- 📈 **Detailed Statistics**: Provides detailed statistics of crawl results.
- ⚡ **Fast Skip Mode**: New `--skip-429` option to skip 429 errors for ultra-fast crawling

## 🚀 New Enhanced Features (v2.0)

- 🧠 **Intelligent 429 Error Handling**: Exponential backoff with time-window tracking
- 💾 **Session Persistence**: Save and resume interrupted crawling sessions
- 📊 **Performance Monitoring**: Real-time performance metrics and optimization suggestions  
- 🔧 **Session Management**: Advanced session analysis, merging, and export tools
- ⚡ **Enhanced Demo Script**: Comprehensive CLI with session recovery capabilities
- 🛠️ **Chrome Driver Compatibility**: Improved browser fallback mechanisms

📖 **[View Detailed Features & Changelog →](FEATURES_AND_CHANGELOG.md)**

## 🚀 Quick Start

### 1. Install Dependencies

```bash
pip install -r requirements.txt
```

### 2. Enhanced Demo (Recommended)

```bash
# Quick test with session saving
python enhanced_demo.py --config quick --save-session

# Production crawl with aggressive rate limiting
python enhanced_demo.py --config production --aggressive-delays --save-session

# Resume interrupted session
python enhanced_demo.py --resume session_20250602_123456

# View help for all options
python enhanced_demo.py --help
```

### 3. Session Management

```bash
# List all available sessions
python session_manager.py list

# Analyze specific session
python session_manager.py analyze demo_20250602_002631

# Merge two sessions
python session_manager.py merge session1 session2 --output merged_session

# Export session data
python session_manager.py export session_id --format csv
```

### 4. Performance Monitoring

```bash
# Monitor crawling performance in real-time
python performance_monitor.py --session-dir output/my_session --interval 2
```

### 5. Quick Commands

```bash
# Quick test
python enhanced_demo.py --config quick --save-session

# Production mode
python enhanced_demo.py --config production --depth 3
python enhanced_demo.py --config quick --no-visualization

# Manual CAPTCHA mode (replaces quick_manual_demo.py)
python enhanced_demo.py --manual-captcha --depth 2 --max-papers 5
```

### 3. Basic Usage

```python
from papertracer import GoogleScholarCrawler, print_citation_tree, save_tree_to_json

# Create crawler instance
crawler = GoogleScholarCrawler(
    max_depth=3,              # Maximum recursion depth
    max_papers_per_level=10,  # Maximum papers per level
    delay_range=(1, 3)        # Request delay range (seconds)
)

# Set starting URL (Google Scholar citation page)
start_url = "https://scholar.google.com/scholar?cites=11002616430871081935&as_sdt=2005&sciodt=0,5&hl=en"

# Build citation tree
citation_tree = crawler.build_citation_tree(start_url)

if citation_tree:
    # Print tree structure
    print_citation_tree(citation_tree)
    
    # Save as JSON file (automatically saved to output/ directory)
    save_tree_to_json(citation_tree, "output/citation_tree.json")
```

### 4. Use Testing Tools

```bash
# Run basic functionality tests
python test_crawler.py --test

# Interactive configuration and testing
python test_crawler.py --interactive

# Show help information
python test_crawler.py
```

## 📊 Data Structure

### Paper Class
```python
@dataclass
class Paper:
    title: str           # Paper title
    authors: str         # Author information
    year: str            # Publication year
    citation_count: int  # Citation count
    url: str             # Paper URL
    cited_by_url: str    # Cited-by page URL
    abstract: str        # Abstract
```

### CitationNode Class
```python
@dataclass 
class CitationNode:
    paper: Paper                    # Paper information
    children: List['CitationNode']  # List of child nodes
    depth: int                      # Node depth
```

## ⚙️ Configuration Parameters

| Parameter | Type | Default | Description |
|----------|------|---------|-------------|
| max_depth | int | 3 | Maximum recursion depth |
| max_papers_per_level | int | 10 | Maximum papers to crawl per level |
| delay_range | tuple | (1, 3) | Request delay range (seconds) |

## 📁 Output Files

### Directory Structure
All output files are saved in the `output/` directory with timestamps to prevent overwriting:

```
output/
├── demo_20250601_143052_citation_tree.json    # Citation data file
├── demo_20250601_143052_simple.png            # Simple network graph
├── demo_20250601_143052_stats.png             # Statistical charts
└── README.md                                  # Output directory README
```

### JSON Format
The generated JSON file contains the complete tree structure:

```json
{
  "paper": {
    "title": "Paper Title",
    "authors": "Author Information",
    "year": "2024",
    "citation_count": 50,
    "url": "Paper URL",
    "cited_by_url": "Cited-by Page URL",
    "abstract": "Abstract"
  },
  "depth": 0,
  "children": [
    {
      "paper": { ... },
      "depth": 1,
      "children": [ ... ]
    }
  ]
}
```

### Visualization Charts
- **Simple Network Graph** (`*_simple.png`): Shows citation relationship network
- **Statistical Charts** (`*_stats.png`): Includes depth distribution, citation count distribution, etc.

## 🔧 Advanced Usage

### Custom Crawler Configuration

```python
# Create custom crawler
crawler = GoogleScholarCrawler(
    max_depth=5,               # Deeper recursion
    max_papers_per_level=20,   # More papers
    delay_range=(2, 5)         # Longer delays
)

# Set custom User-Agent
crawler.session.headers.update({
    'User-Agent': 'Your Custom User Agent'
})
```

### Batch Process Multiple Links

```python
urls = [
    "https://scholar.google.com/scholar?cites=11002616430871081935&as_sdt=2005&sciodt=0,5&hl=en",
    "https://scholar.google.com/scholar?cites=ANOTHER_ID&as_sdt=2005&sciodt=0,5&hl=en"
]

for i, url in enumerate(urls):
    tree = crawler.build_citation_tree(url)
    if tree:
        save_tree_to_json(tree, f"output/citation_tree_{i}.json")
```

### Visualization Tools

```bash
# Create all visualization types
python visualize_tree.py output/demo_20250601_143052_citation_tree.json --type all

# Create only simple network graph
python visualize_tree.py output/demo_20250601_143052_citation_tree.json --type simple

# Specify output path
python visualize_tree.py output/demo_20250601_143052_citation_tree.json --output output/my_visualization
```

### Interactive HTML Visualization

```bash
# Create interactive HTML visualization
python html_visualizer.py output/demo_20250601_143052_citation_tree.json --output output/interactive.html

# Open generated HTML file in browser
open output/interactive.html  # macOS
# or
start output/interactive.html  # Windows
# or
xdg-open output/interactive.html  # Linux
```

**HTML Visualization Features:**
- 🖱️ Mouse hover shows paper details (title, authors, year, citation count, abstract)
- 🔗 Click nodes to access original paper links
- 🎛️ Supports tree and force-directed layouts
- 🔍 Supports zoom and drag operations
- 📊 Shows real-time statistics and depth legend
- 🎨 Modern UI design
- 📱 Responsive layout for different screen sizes

## ⚠️ Important Notes

1. **Compliance**: Please comply with Google Scholar's terms of use and robots.txt
2. **Request Frequency**: Avoid excessive requests, set appropriate delays
3. **Data Volume Control**: Large-scale crawling may take significant time
4. **Network Stability**: Ensure stable network connection
5. **Anti-Crawling Mechanisms**: If encountering access limits, increase delays or change User-Agent

## 🐛 Frequently Asked Questions

### Q: Crawler returns empty results?
A: Possible reasons:
- Incorrect URL format
- Network connection issues
- Google Scholar temporary access restrictions
- Paper has no citation records

### Q: How to get Google Scholar citation links?
A: 
1. Search for target paper in Google Scholar
2. Click "Cited by X" link below the paper
3. Copy URL from browser address bar

### Q: Crawling is too slow?
A: Try:
- Reducing `delay_range` values (but beware of anti-crawling)
- Reducing `max_papers_per_level`
- Reducing `max_depth`

## 📈 Performance Recommendations

- **Small-scale testing**: For first use, set `max_depth=2`, `max_papers_per_level=5`
- **Medium-scale**: `max_depth=3`, `max_papers_per_level=10`
- **Large-scale crawling**: Process in batches with longer delays

## 🤝 Contribution

Welcome to submit Issues and Pull Requests to improve this project!

## 📄 License

This project is for academic research and personal learning only. Commercial use is prohibited.