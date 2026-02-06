################################################################################
# Project:  OGR SXF driver
# Purpose:  CMake build scripts
################################################################################
# Copyright (c) 2013-2026, NextGIS <info@nextgis.com>
#
# Permission is hereby granted, free of charge, to any person obtaining a
# copy of this software and associated documentation files (the "Software"),
# to deal in the Software without restriction, including without limitation
# the rights to use, copy, modify, merge, publish, distribute, sublicense,
# and/or sell copies of the Software, and to permit persons to whom the
# Software is furnished to do so, subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included
# in all copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS
# OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL
# THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
# FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER
# DEALINGS IN THE SOFTWARE.
################################################################################

cmake_minimum_required(VERSION 3.17)

project(ogr_SXF
    VERSION 1.0.0
    LANGUAGES C CXX
    DESCRIPTION "SXF OGR driver plugin for GDAL"
)

set(SXF_VERSION ${PROJECT_VERSION})

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake/borsch/cmake/")

include(FindAnyProject)
find_anyproject(GDAL REQUIRED)

set(SXF_SOURCES
    src/ogrsxfdatasource.cpp
    src/ogrsxflayer.cpp
    src/ogrsxfdriver.cpp
    src/sxffile.cpp
    src/rscfile.cpp
    src/util.cpp
    src/ogrmemlayer.cpp
)

set(SXF_HEADERS
    src/ogr_sxf.h
    src/ogr_mem.h
    src/org_sxf_defs.h
)

add_library(ogr_SXF MODULE ${SXF_SOURCES} ${SXF_HEADERS})

set_target_properties(ogr_SXF PROPERTIES
    PREFIX ""
    OUTPUT_NAME "ogr_SXF"
    WINDOWS_EXPORT_ALL_SYMBOLS ON
    VERSION ${SXF_VERSION}
)

target_include_directories(ogr_SXF PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/src
)

target_link_extlibraries(ogr_SXF)

if(NOT SKIP_INSTALL_LIBRARIES AND NOT SKIP_INSTALL_ALL)
    install(TARGETS ogr_SXF
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}/gdalplugins
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    )
endif()

if(NOT DEFINED PACKAGE_VENDOR)
    set(PACKAGE_VENDOR NextGIS)
endif()

include(${CMAKE_CURRENT_LIST_DIR}/cmake/borsch/cmake/packaging.cmake)
create_borsch_package(${PROJECT_NAME}
    VENDOR ${PACKAGE_VENDOR}
    VERSION ${SXF_VERSION}
    TARGETS ogr_SXF
    HEADERS ${SXF_HEADERS}
)