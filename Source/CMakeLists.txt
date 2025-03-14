
add_executable(WBOARD WIN32 
)


if(MSVC)
    set_target_properties(WBOARD PROPERTIES
        VS_DEBUGGER_ENVIRONMENT "PATH=C:/Qt/Qt5.9.5/5.9.5/msvc2017_64/plugins/platforms \
                                 C:/Qt/Qt5.9.5/5.9.5/msvc2017_64/bin;$ENV{PATH}"
    )
endif()

file(GLOB_RECURSE SOURCES "${CMAKE_SOURCE_DIR}/Source/*.cpp" "${CMAKE_SOURCE_DIR}/Source/*.h")
file(GLOB_RECURSE UI "${CMAKE_SOURCE_DIR}/UI/*.ui")
file(GLOB_RECURSE RC "${CMAKE_SOURCE_DIR}/Resources/WBoard.qrc" "${CMAKE_SOURCE_DIR}/WBoard.rc")


# 可选：自动按目录分组
foreach(_source IN ITEMS ${SOURCES})
    get_filename_component(_source_path "${_source}" PATH)
    file(RELATIVE_PATH _source_path_rel "${CMAKE_SOURCE_DIR}" "${_source_path}")
    string(REPLACE "/" "\\" _group_path "${_source_path_rel}")
    source_group("${_group_path}" FILES "${_source}")
endforeach()


target_sources(WBOARD PRIVATE ${SOURCES})
target_sources(WBOARD PRIVATE ${UI})
target_sources(WBOARD PRIVATE ${RC})


target_include_directories(WBOARD PRIVATE ${CMAKE_SOURCE_DIR}/ThirdParty/quazip/include)
target_include_directories(WBOARD PRIVATE ${CMAKE_SOURCE_DIR}/ThirdParty/openssl/include)

target_link_libraries(WBOARD PRIVATE
  debug  ${CMAKE_SOURCE_DIR}/ThirdParty/quazip/Debug/lib/quazipd.lib
  optimized  ${CMAKE_SOURCE_DIR}/ThirdParty/quazip/Release/lib/quazip.lib
  ${CMAKE_SOURCE_DIR}/ThirdParty/openssl/lib/libssl.lib
  ${CMAKE_SOURCE_DIR}/ThirdParty/openssl/lib/libcrypto.lib
  Qt5::Network
  Qt5::Sql
  Qt5::Widgets
  Qt5::Xml
  Qt5::XmlPatterns
  Qt5::Svg
  Qt5::Qml
  Qt5::Multimedia
  Qt5::MultimediaWidgets
  Qt5::WebEngine
  Qt5::WebEngineWidgets
  Qt5::WebEngineCore
  Qt5::PrintSupport
  Qt5::UiTools
)

target_compile_definitions(WBOARD PRIVATE WBOARD_VERSION="${WBOARD_VERSION}")

if(WIN32)
  # Windows resource
  string(REPLACE . , PRODUCT_VERSION ${PROJECT_VERSION})

  target_sources(WBOARD PRIVATE ${CMAKE_SOURCE_DIR}/WBoard.rc)

  if(WINDOWS_DEPLOY)
    install(TARGETS WBOARD RUNTIME DESTINATION ${CMAKE_INSTALL_PREFIX})
  else()
    target_compile_definitions(WBOARD PRIVATE NODEPLOY)
    install(TARGETS WBOARD RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})
  endif()

  install(DIRECTORY qml DESTINATION ${CMAKE_INSTALL_PREFIX}/share/WBOARD/)
  install(DIRECTORY ${CMAKE_SOURCE_DIR}/filter-sets DESTINATION ${CMAKE_INSTALL_PREFIX}/share/WBOARD/)
else()
    target_sources(WBOARD PRIVATE widgets/openglvideowidget.h widgets/openglvideowidget.cpp)
endif()
