# Infrastructure Analyzer — User Guide

Complete step-by-step guide to using all features of the application.

---

## Table of Contents

1. [Installation](#1-installation)
2. [Application Overview](#2-application-overview)
3. [Location Tab — Navigating the Map](#3-location-tab--navigating-the-map)
4. [Fetching Live Data from OpenStreetMap](#4-fetching-live-data-from-openstreetmap)
5. [Layers Tab — Controlling the Map Display](#5-layers-tab--controlling-the-map-display)
6. [Search Tab — Finding Infrastructure Nodes](#6-search-tab--finding-infrastructure-nodes)
7. [Node Details Panel](#7-node-details-panel)
8. [Analysis Tab — Running Infrastructure Analysis](#8-analysis-tab--running-infrastructure-analysis)
9. [Console Tab — Viewing Results](#9-console-tab--viewing-results)
10. [Importing & Exporting Data](#10-importing--exporting-data)
11. [Generating Reports](#11-generating-reports)
12. [File Menu](#12-file-menu)
13. [Keyboard Shortcuts](#13-keyboard-shortcuts)
14. [Troubleshooting](#14-troubleshooting)

---

## 1. Installation

### System Requirements

- **OS**: Windows 10/11, Linux, macOS
- **Python**: 3.10 or higher
- **RAM**: 4 GB minimum (8 GB recommended for large datasets)
- **Display**: 1280x720 minimum (1920x1080 recommended)

### Step 1: Install Python

Download from https://www.python.org/downloads/ — ensure "Add Python to PATH" is checked during installation.

### Step 2: Set Up Virtual Environment (Recommended)

```bash
# Windows
python -m venv .venv
.venv\Scripts\activate

# Linux / macOS
python3 -m venv .venv
source .venv/bin/activate
```

### Step 3: Install Dependencies

```bash
pip install -r requirements.txt
```

### Step 4: Run the Application

```bash
python main.py
```

---

## 2. Application Overview

The main window is divided into four areas:

```
┌──────────────────────────────────────────────────────────────┐
│  Menu Bar (File | Analysis | View | Help)                    │
├──────────────────────────────────────────────────────────────┤
│  Toolbar (Import | Export | Analyze | Report | Refresh | Fit)│
├──────────┬────────────────────────────────────┬──────────────┤
│          │                                    │              │
│  LEFT    │      CENTRAL MAP VIEW              │  RIGHT       │
│  PANEL   │      (Folium Interactive Map)      │  DOCK        │
│          │                                    │  (Node       │
│  Tabs:   │   • Click markers for popups       │  Details)    │
│  Location│   • Scroll to zoom                  │              │
│  Layers  │   • Drag to pan                     │  • Category  │
│  Search  │   • Layer control (top-right)       │  • Coord.    │
│  Analysis│   • Scale bar (bottom-left)         │  • Vuln.     │
│  Console │   • Measure tool (bottom-left)      │  • Central.  │
│          │                                    │  • Attributes │
├──────────┴────────────────────────────────────┴──────────────┤
│  Status Bar (Node count, edge count, category count)          │
└──────────────────────────────────────────────────────────────┘
```

### Interface Components

| Component | Purpose |
|---|---|
| **Left Panel** | 5 tabs: Location, Layers, Search, Analysis, Console |
| **Central Map** | Interactive Folium map with all infrastructure markers |
| **Right Dock** | Detailed information about the selected node |
| **Menu Bar** | File, Analysis, View, Help menus |
| **Toolbar** | Quick-access buttons for common actions |
| **Status Bar** | Network statistics and current status messages |

---

## 3. Location Tab — Navigating the Map

The Location tab (first tab in the left panel) is your starting point.

### Searching for a Location

1. Type a city, country, or address in the **Search Location** field
   - Examples: `Islamabad`, `Tokyo, Japan`, `Times Square, New York`
2. Press **Enter** or click **Go to Location**
3. The map will center on the found location
4. A popup will ask: *"Fetch infrastructure data for this location from OpenStreetMap?"*
   - Click **Yes** to automatically fetch live data
   - Click **No** to just view the map

### Using Coordinates

If you know the exact latitude and longitude:

1. Enter latitude in the **Lat** field (e.g., `33.6844`)
2. Enter longitude in the **Lon** field (e.g., `73.0479`)
3. Adjust the **Zoom Level** slider (3 = far, 18 = very close)
4. Click **Go to Coordinates**

### Understanding the Status

Below the controls, the **Status** label shows:
- `Ready` — waiting for input
- `Searching for '...'` — geocoding in progress
- `Location: ...` — current map center
- `Fetching OSM data...` — downloading from OpenStreetMap
- `Loaded X OSM infrastructure points` — success message
- Error messages in red if something goes wrong

---

## 4. Fetching Live Data from OpenStreetMap

The application can fetch real infrastructure data from OpenStreetMap using the Overpass API.

### Automatic Prompt

After navigating to a location, you will be asked:
- **No data loaded yet**: *"Fetch infrastructure data for 'Islamabad' from OpenStreetMap?"*
- **Data already loaded**: *"Switch to 'Islamabad' and fetch fresh data? (Currently loaded: X nodes from OSM - Old Location)"*

### Manual Fetch

Click the **Fetch OSM Data** button in the Location tab at any time to re-fetch for the current map center.

### Configuration Options

| Setting | Description |
|---|---|
| **Category Filter** | Choose what types of infrastructure to fetch: All, Transportation, Energy & Utilities, Healthcare, Education, Government |
| **Search Radius** | Search radius in kilometers (0.05 km to 50 km). Default: 2 km |

### What Gets Fetched

The following 14 infrastructure types are queried:

| Type | OSM Tag | Category |
|---|---|---|
| Airports, Heliports | `aeroway` | Transportation |
| Railway Stations | `railway` | Transportation |
| Bus & Ferry Terminals | `amenity` | Transportation |
| Ports & Harbours | `seamark:type` | Transportation |
| Power Plants & Substations | `power` | Energy |
| Water Treatment Plants | `man_made` | Water |
| Hospitals & Clinics | `amenity` | Healthcare |
| Schools & Universities | `amenity` | Educational |
| Government Buildings | `amenity` | Government |
| Government Offices | `office` | Government |
| Fuel Stations | `amenity` | Energy |
| Telecom Infrastructure | `telecom` | Communication |
| Communications | `communications` | Communication |
| Warehouses & Storage | `industrial` | Logistics |

### Data Usage Note

OpenStreetMap data is © OpenStreetMap contributors and available under the Open Database License (ODbL).

---

## 5. Layers Tab — Controlling the Map Display

The Layers tab controls what is shown on the map.

### Infrastructure Layers

Each infrastructure category can be toggled on/off by checking/unchecking its box:
- Government & Administrative
- Transportation Hub
- Energy Facility
- Communication Center
- Healthcare Facility
- Logistics & Warehouse
- Water Infrastructure
- Emergency Services
- Educational Institution
- Other

Two additional layers:
- **Dependencies** — Shows connection lines between infrastructure nodes (requires imported data with dependency edges)
- **Heat Map** — Overlays a heat map showing node density

When a layer is toggled, the map automatically refreshes.

### Base Map Styles

Use the **Base Map** dropdown to switch between 5 tile layers:

| Style | Best For |
|---|---|
| **Street Map** | Default OpenStreetMap view — good for general use |
| **Satellite** | High-resolution aerial imagery from Esri |
| **Terrain** | Topographic map with elevation shading |
| **Dark Mode** | Low-light environments, presentations |
| **Hybrid Satellite** | Satellite imagery with road labels |

---

## 6. Search Tab — Finding Infrastructure Nodes

The Search tab helps you locate specific infrastructure nodes.

### Searching

1. Type a name or keyword in the **Search Nodes** field
   - Matches node names and category names
   - Results update as you type (real-time filtering)
2. Use **Filter by Category** to narrow by infrastructure type

### Results List

Matching nodes appear in the list showing:
- Node name
- Category (in parentheses)

### Selecting a Node

- **Click** any result → right panel shows full details
- **Double-click** or use the popup on the map marker

### Clearing Results

Click the **Clear Results** button to reset the search.

---

## 7. Node Details Panel

The right-side panel shows detailed information about the selected node.

### Information Displayed

| Field | Description |
|---|---|
| **Node Title** | Name of the selected infrastructure asset |
| **Category** | Infrastructure type (color-coded) |
| **Subcategory** | More specific classification |
| **Latitude** | Decimal degrees with hemisphere (N/S) |
| **Longitude** | Decimal degrees with hemisphere (E/W) |
| **Precision** | ±0.11 meters (at equator) |
| **Vulnerability** | Score 0–1 with risk level bar (LOW/MEDIUM/HIGH) |
| **Centrality** | Network centrality score (requires analysis) |
| **Attributes** | Additional properties (osm_id, emergency status, etc.) |
| **Source** | Data source (e.g., OpenStreetMap) |
| **Node ID** | Unique identifier |

### Center Map on Node

Click the **Center Map on Node** button to re-center the map on the currently selected node.

### Accessing Details

Node details can be accessed by:
- **Clicking a map marker** (the marker labels are clickable)
- **Clicking a search result** in the Search tab
- **Viewing the popup** when clicking a marker on the map

---

## 8. Analysis Tab — Running Infrastructure Analysis

The Analysis tab contains all analytical tools.

### Vulnerability Assessment

Evaluates every node on four dimensions:
- **Centrality** — How central is the node in the dependency graph
- **Dependency Impact** — How many downstream nodes are affected if this fails
- **Category Risk** — Base risk of the infrastructure type
- **Connectivity** — Number of connections relative to network average

Results are shown in the Console tab with:
- Mean vulnerability score
- High/Medium/Low risk counts
- Top 5 most vulnerable nodes

### Category Distribution

Shows the count and percentage of each infrastructure category, with visual bar charts in the console.

### Cascading Failure Simulation

Models how the failure of one infrastructure asset propagates:

1. **Select a node** from the dropdown — this will be the initially failing node
2. **Set Failure Threshold** (0.1–1.0) — proportion of upstream dependencies that must fail before a node is affected
   - Lower threshold (e.g., 0.3) = more aggressive propagation
   - Higher threshold (e.g., 0.8) = more conservative
3. Click **Simulate Cascade Failure**

Results show:
- Initial failure node and category
- Total affected nodes
- Propagation depth (steps)
- Step-by-step cascade path

### Service Area Analysis

Calculates coverage overlap within a given radius:

1. Set **Analysis Radius** (0.1–50 km)
2. Click **Calculate Service Areas**

Results show:
- Average neighbors within radius
- Isolated nodes (no neighbors)
- Highest coverage overlap areas

### Emergency Response Time

Estimates travel time from an emergency location to the nearest facility:

1. A dialog appears asking for:
   - **Latitude/Longitude** of the emergency
   - **Facility Type** (default: Healthcare)
   - **Transport Mode** (Ambulance: 60 km/h, Fire Truck: 50 km/h, Police: 65 km/h, On Foot: 5 km/h)
2. Results show nearest facility, distance, and estimated response time

### Comprehensive Report

Runs all analyses at once and generates a combined result displayed in the Console tab.

---

## 9. Console Tab — Viewing Results

The Console tab displays all analysis results and application messages.

### Features

- Timestamped log messages
- Color-coded output sections
- Scrollable history
- **Clear** button to reset
- **Export to File** button to save console output as `.txt`

### Reading Results

Each analysis produces a formatted output:
```
══════════════════════════════════════════════
Analysis: Vulnerability Assessment
══════════════════════════════════════════════
  Vulnerability Assessment: OSM - Islamabad
  Analyzed 45 infrastructure nodes across 6 categories.
  ...
Top scores:
  Islamabad International Airport: 0.720
  ...
══════════════════════════════════════════════
```

---

## 10. Importing & Exporting Data

### Importing Files

Supported formats: **GeoJSON**, **CSV**, **KML**

From the **File** menu or **Import** toolbar button:

1. Select **File > Import GeoJSON** (Ctrl+O), **Import CSV**, or **Import KML**
2. Browse to your file
3. The data is loaded, classified, and displayed on the map

#### CSV Format Requirements

For CSV files, columns are auto-detected:

| Required | Column Name Examples |
|---|---|
| Latitude | `lat`, `latitude`, `y`, `Latitude` |
| Longitude | `lon`, `lng`, `longitude`, `x`, `Longitude` |
| Name (optional) | `name`, `Name`, `title` |
| Category (optional) | `category`, `type`, `Category` |
| Subcategory (optional) | `subcategory`, `Subcategory` |

If no category column is found, nodes are auto-classified based on keywords in their name or attributes.

### Exporting Files

Supported formats: **GeoJSON** (Ctrl+S), **CSV**, **KML**

From the **File** menu or **Export** toolbar button:

1. Select format
2. Choose save location
3. File is saved with all current node data including coordinates, categories, and attributes

### Exporting the Map

- **Export Map HTML** — Saves the current map as a standalone interactive HTML file that can be opened in any web browser

---

## 11. Generating Reports

### Export Report (PDF/Text)

From **File > Export Report (PDF)** or the **Report** toolbar button:

1. If no data is loaded, you'll be prompted to load data first
2. A save dialog appears — choose `.pdf` or `.txt`
3. Analysis runs automatically (vulnerability, centrality, distribution)
4. Report is generated and saved

### Report Contents (PDF)

| Section | Content |
|---|---|
| **Cover Page** | Report title, timestamp, node count, coordinate precision |
| **Executive Summary** | Vulnerability assessment summary, risk distribution |
| **Key Metrics Dashboard** | Total nodes, high/medium/low risk counts, mean/max vulnerability |
| **Node Scores** | Ranked list of all nodes with scores and risk levels |
| **Infrastructure Inventory** | Full table of every node with coordinates (DMS and DD) |
| **Per-Node Details** | Detailed breakdown for each node (up to 300 nodes) |
| **Vulnerability Ranking** | All nodes sorted by vulnerability score with coordinates |
| **Category Distribution** | Each category with geographic center, range, and node list |
| **Cascade Analysis** | If cascade simulation was run, the propagation chain |
| **Location Summary** | Network center, bounding box, lat/lon spans |
| **Accuracy Statement** | Coordinate precision certification |

### Report Contents (Text)

If PDF generation fails (e.g., on systems without the fpdf2 library), a text report is generated automatically with all the same data.

### Exporting Console Output

From the Console tab, click **Export to File** to save the current console log as a `.txt` file.

---

## 12. File Menu

| Menu Item | Shortcut | Description |
|---|---|---|
| **File > Import GeoJSON** | Ctrl+O | Import data from GeoJSON file |
| **File > Import CSV** | — | Import data from CSV file |
| **File > Import KML** | — | Import data from KML file |
| **File > Export GeoJSON** | Ctrl+S | Export data to GeoJSON |
| **File > Export CSV** | — | Export data to CSV |
| **File > Export KML** | — | Export data to KML |
| **File > Export Map HTML** | — | Save interactive map as HTML |
| **File > Export Report (PDF)** | — | Generate and save analysis report |
| **File > Quick Start Guide** | — | View this guide within the app |
| **File > Exit** | Ctrl+Q | Close the application |

| **Analysis > Vulnerability Assessment** | — | Run vulnerability analysis |
| **Analysis > Cascading Failure Simulation** | — | Run cascade simulation |
| **Analysis > Service Area Analysis** | — | Run service area analysis |
| **Analysis > Emergency Response Time** | — | Estimate response times |
| **Analysis > Category Distribution** | — | Show category breakdown |
| **Analysis > Comprehensive Report** | — | Run all analyses |

| **View > Zoom to Fit All Nodes** | — | Auto-zoom to show all nodes |
| **View > Reload Map** | — | Refresh the map display |

| **Help > About** | — | Application information |
| **Help > Documentation** | — | Quick start guide |

---

## 13. Keyboard Shortcuts

| Shortcut | Action |
|---|---|
| **Ctrl+O** | Import GeoJSON |
| **Ctrl+S** | Export GeoJSON |
| **Ctrl+Q** | Exit application |
| **Enter** (in Location field) | Go to location |
| **Mouse wheel** | Zoom in/out on map |
| **Click + drag** on map | Pan the map |

---

## 14. Troubleshooting

### Map Shows Black at High Zoom

Zoom level is capped at 18. If the map is black at zoom 18, the tile server may not have tiles for that location at that zoom level. Try switching to a different tile layer (e.g., Street Map instead of Satellite).

### OSM Fetch Returns No Data

Possible causes:
- The search radius is too small for the area (try increasing to 5+ km)
- The category filter is too restrictive (try "All Infrastructure")
- The area is not well-mapped on OpenStreetMap
- Network connectivity issues (check your internet connection)
- Overpass API rate limiting (wait a minute and try again)

### OSM Fetch Error: "Connection aborted"

This is typically a network/SSL issue. Check:
- Your internet connection
- Corporate firewall or proxy settings
- Try again after a few seconds

### "python has stopped working" When Generating Report

The crash has been handled — the application will now fall back to generating a text report automatically. The PDF generation may fail with very large datasets (1000+ nodes). Use the text format for large networks.

### App Shows Wrong Location Data

Make sure you have fetched OSM data for the current location:
1. Navigate to the correct location using the Location tab
2. When prompted, click **Yes** to fetch fresh data
3. Verify the console shows the correct location name

### Map Markers Not Showing Details in Right Panel

Click directly on the marker's **name label** (the white text box below the colored dot on the map). The details will appear in the right panel.

### Slow Performance with Large Datasets

- Reduce the search radius when fetching OSM data
- Use category filters to fetch fewer nodes
- Toggle off Heat Map and Dependencies layers
- Switch to a simpler tile layer (Street Map)

### Tile Layer Not Loading

Some tile servers have usage limits:
- Satellite and Hybrid tiles require internet access to the Esri servers
- If a layer doesn't load, switch to "Street Map" which is most reliable

### Application Won't Start

Ensure all dependencies are installed:
```bash
pip install -r requirements.txt
```

If using a virtual environment, verify it's activated before running `python main.py`.

### "No module named 'PyQt5'" Error

Install PyQt5:
```bash
pip install PyQt5 PyQtWebEngine
```

---

*Developed by Iran Govt*
