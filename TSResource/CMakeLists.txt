find_package(Qt5 REQUIRED COMPONENTS LinguistTools)

# Translation howto:
# - Translations are mentioned in the LANGUAGES variable below
# - To update the .ts files, cd into translations and run 'ninja translations'
# - The .qm files are created as part of a regular make command

set(LANGUAGES
    en
    zh_CN
)

list(TRANSFORM LANGUAGES APPEND ".ts" OUTPUT_VARIABLE TS_FILES)
list(TRANSFORM LANGUAGES APPEND ".qm" OUTPUT_VARIABLE QM_FILES)


#lupdate ts files
file(GLOB_RECURSE SOURCES "${CMAKE_SOURCE_DIR}/Source/*")
qt5_create_translation(QM_FILES "${SOURCES}" ${TS_FILES})
add_custom_target(qm ALL DEPENDS ${QM_FILES})



if(UNIX AND NOT APPLE)
  include(GNUInstallDirs)
  install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/ DESTINATION ${CMAKE_INSTALL_DATADIR}/shotcut/translations
    FILES_MATCHING PATTERN "*.qm" PATTERN CMakeFiles EXCLUDE)
endif()