# Creating and installng modules
Structure folder:
```
moduleName
|_ CMakeLists.txt
|_ pyproject.toml
|_ moduleName
||_ __init__.py
|_ src
||_ files.h
||_ bindings.cpp
```
The __init__.py shold contain something like:
```
from .moduleName import *
```
Example CMakeLists.txt:
```
cmake_minimum_required(VERSION 3.15)
project(moduleName LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_POSITION_INDEPENDENT_CODE ON)

find_package(pybind11 REQUIRED)
include_directories(/opt/homebrew/include)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/src)

pybind11_add_module(moduleName MODULE src/reb_bindings.cpp)
set_target_properties(moduleName PROPERTIES
    LIBRARY_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/moduleName"
)
```
Example pyproject.toml
```
[build-system]
requires = ["scikit-build-core", "pybind11", "numpy"]
build-backend = "scikit_build_core.build"

[project]
name = "moduleName"
version = "0.1.0"
dependencies = ["numpy"]

[tool.scikit-build]
build.verbose = true
wheel.expand-macos-universal-tags = true
```
Go to folder main folder moduleName, compile and install:
```
pip install . -v
```
