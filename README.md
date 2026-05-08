# Infrastructure Analyzer

A comprehensive geospatial infrastructure analysis tool for urban planning, emergency management, and critical infrastructure assessment. Features live OpenStreetMap data integration, multi-layer interactive mapping, vulnerability analysis, cascading failure simulation, and professional PDF reporting with sub-meter coordinate accuracy.

---

## Features

- **Live OSM Data** — Fetch real infrastructure points from OpenStreetMap for any city worldwide (airports, railways, hospitals, power plants, schools, government buildings, telecom, and more)
- **Interactive Map** — Folium-based with 5 tile layers (Street, Satellite, Terrain, Dark, Hybrid), category-colored markers with always-visible labels, heat maps, and service radius overlays
- **Vulnerability Assessment** — Multi-factor scoring based on centrality, dependency impact, category risk, and connectivity
- **Cascading Failure Simulation** — BFS-based propagation modeling with configurable thresholds
- **Service Area Analysis** — KD-tree spatial indexing for coverage overlap and gap detection
- **Emergency Response Time** — Haversine-based distance and travel time estimation for multiple transport modes
- **Comprehensive Reporting** — PDF and text reports with 6-decimal-place coordinates (DMS + DD), per-node details, vulnerability rankings, category distribution, and geographic extent
- **Import/Export** — GeoJSON, CSV, KML with keyword-based auto-classification

---

## Quick Start

### Prerequisites

- Python 3.10+
- pip

### Installation

```bash
# Clone the repository
git clone https://github.com/your-org/infrastructure-analyzer.git
cd infrastructure-analyzer

# Install dependencies
pip install -r requirements.txt

# Run the application
python main.py
```

### First Use

1. Go to the **Location** tab
2. Search for a city or country (e.g., "Tokyo", "London", "Islamabad")
3. Click **Go to Location** or press Enter
4. Click **Yes** when prompted to fetch OSM data
5. Explore the map — click any marker to view node details in the right panel
6. Run analyses from the **Analysis** tab
7. Export reports via **File > Export Report**

---

## Project Structure

```
infrastructure_analyzer/
├── main.py                  # Application entry point
├── requirements.txt         # Python dependencies
├── README.md
├── src/
│   ├── __init__.py
│   ├── gui.py               # PyQt5 interface, OSM fetch, geocoding, all controls
│   ├── models.py            # InfrastructureCategory, InfrastructureNode, InfrastructureNetwork, AnalysisResult
│   ├── map_renderer.py      # Folium map, markers, tile layers, DivIcon labels
│   ├── data_handler.py      # GeoJSON/CSV/KML import/export, classification
│   ├── analysis_engine.py   # Vulnerability, cascade, service area, centrality
│   └── report_generator.py  # PDF/text reports with coordinate precision
├── data/
│   └── sample/              # Sample GeoJSON for testing
├── tests/
│   ├── test_core.py         # Functional test suite
│   └── test_osm_api.py      # OSM API debug tests
└── docs/
```

---

## How It Works

### Data Flow

1. **Geocoding** — User enters a location name → Nominatim API → lat/lon
2. **OSM Fetch** — Overpass API query for 14 infrastructure categories within a configurable radius
3. **Classification** — Each OSM element is classified into an `InfrastructureCategory` (10 types) based on its tags
4. **Mapping** — Nodes are rendered as colored markers with DivIcon name labels on a Folium map
5. **Analysis** — NetworkX graph is built; vulnerability, cascade, and service area algorithms run against it
6. **Reporting** — Results are compiled into PDF or text reports with full coordinate detail

### Coordinate Precision

All coordinates are reported at **6 decimal places** (WGS84 datum), providing approximately **±0.11 meters** accuracy at the equator. Both Decimal Degrees (DD) and Degrees/Minutes/Seconds (DMS) formats are included in reports.

### Infrastructure Categories

| Category | Color | Examples |
|---|---|---|
| Government & Administrative | Dark Red | Town halls, courthouses, police stations |
| Transportation Hub | Blue | Airports, railway stations, bus terminals |
| Energy Facility | Orange | Power plants, substations, fuel stations |
| Communication Center | Purple | Telecom towers, communications infrastructure |
| Healthcare Facility | Red | Hospitals, clinics |
| Logistics & Warehouse | Dark Green | Warehouses, industrial storage |
| Water Infrastructure | Teal | Water treatment, wastewater plants |
| Emergency Services | Orange-Red | Fire stations, emergency response |
| Educational Institution | Green | Schools, universities, colleges |

---

## Analysis Methods

### Vulnerability Assessment

Composite score (0–1) calculated from:
- **Centrality (30%)** — Degree, betweenness, and closeness centrality in the dependency graph
- **Dependency Impact (30%)** — Number of downstream nodes affected by failure
- **Category Risk (20%)** — Base risk level of the infrastructure type
- **Connectivity (20%)** — Degree relative to network average

### Cascading Failure

BFS-based simulation with configurable failure threshold. Propagates through dependency edges — a node fails when the proportion of failed upstream dependencies exceeds the threshold.

### Service Area Analysis

KD-tree spatial indexing (via SciPy) identifies all nodes within a given radius, computing coverage overlap, isolated nodes, and redundancy statistics.

### Emergency Response Time

Haversine great-circle distance calculation combined with configurable transport speeds:
- Ambulance: 60 km/h
- Fire Truck: 50 km/h
- Police: 65 km/h
- On Foot: 5 km/h

---

## Dependencies

- **PyQt5 / PyQtWebEngine** — Desktop GUI and embedded web view
- **Folium** — Interactive map rendering (Leaflet.js)
- **NetworkX** — Graph-based dependency and centrality analysis
- **NumPy / SciPy** — Numerical computation, KD-tree spatial indexing
- **fpdf2** — PDF report generation
- **Pandas / GeoPandas / Shapely** — Geospatial data handling
- **Matplotlib** — Visualization support
- **Branca** — Folium color/attribute utilities

---

## Sample Data

A sample GeoJSON file is provided at `data/sample/sample_infrastructure.geojson` for testing. Import it via **File > Import GeoJSON**.

---

## License

Developed by Iran Govt.

---

## Contributing

Internal project. For inquiries, please contact the development team.
