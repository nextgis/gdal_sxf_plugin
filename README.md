# SXF OGR Driver

This repository builds the SXF (Storage and eXchange Format) OGR driver as an external GDAL plugin.

## Overview

The SXF format is a Russian vector data exchange format commonly used for cadastral and cartographic data. This driver provides read support for SXF files through GDAL/OGR.

The `src` folder contains a standalone copy of the SXF driver sources plus a minimal OGR memory layer implementation required by the driver.

## Requirements

- CMake 3.10 or later
- C++11 compatible compiler
- GDAL library (2.0 or later)

## Building

The project uses the NextGIS borsch build system:

```bash
mkdir build
cd build
cmake ..
cmake --build .
```

### CMake Options

- `WITH_GDAL` - Enable/disable GDAL dependency (default: ON)
- `WITH_GDAL_EXTERNAL` - Use external GDAL from NextGIS repository (default: OFF)
- `BUILD_SHARED_LIBS` - Build shared libraries (default: ON)

## Installation

```bash
cmake --install .
```

The plugin will be installed to `<prefix>/lib/gdalplugins/ogr_SXF.so` (or `.dll` on Windows).

## Usage

After installation, GDAL will automatically detect and load the SXF driver:

```bash
ogrinfo -ro your_file.sxf
```
## Support

For questions and support, please contact NextGIS: <info@nextgis.com>