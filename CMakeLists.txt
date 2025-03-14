cmake_minimum_required(VERSION 3.13)
if (${CMAKE_VERSION} VERSION_GREATER_EQUAL "3.20")
  cmake_policy(SET CMP0115 NEW)
endif()

if(NOT DEFINED WBOARD_VERSION)
  string(TIMESTAMP WBOARD_VERSION "%y.%m.%d")
endif()

project(WBOARD
  VERSION ${WBOARD_VERSION}
  LANGUAGES CXX
)
set(CMAKE_AUTOUIC_SEARCH_PATHS ${CMAKE_SOURCE_DIR}/UI)
if(WIN32)
  option(WINDOWS_DEPLOY "Install exes/libs directly to prefix (no subdir /bin)" ON)
endif()

include(FeatureSummary)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_INCLUDE_CURRENT_DIR ON)



find_package(Qt5  REQUIRED
  COMPONENTS
  Network
  Sql
  Widgets
  Xml
  XmlPatterns
  Svg
  Qml
  UiTools
  Multimedia
  MultimediaWidgets
  WebEngine 
  WebEngineCore
  WebEngineWidgets
  PrintSupport
)                              



add_subdirectory(Source)
add_subdirectory(TSResource)

feature_summary(WHAT ALL INCLUDE_QUIET_PACKAGES FATAL_ON_MISSING_REQUIRED_PACKAGES)
