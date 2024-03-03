set(TARGET_NAME RapidXML)

cmake_minimum_required(VERSION 3.16)

project(RapidXML
	LANGUAGES C CXX
	VERSION "1.16"
)

file(GLOB_RECURSE SOURCE_LIST CONFIGURE_DEPENDS RapidXML/RapidXML.aps RapidXML/RapidXML.rc RapidXML/RapidXMLSTD.cpp RapidXML/resource.h)
file(GLOB_RECURSE HEADER_LIST CONFIGURE_DEPENDS RapidXML/rapidxml.hpp RapidXML/rapidxml_iterators.hpp RapidXML/rapidxml_print.hpp RapidXML/rapidxml_utils.hpp RapidXML/RapidXMLSTD.hpp)

add_library(${TARGET_NAME} STATIC ${SOURCE_LIST} ${HEADER_LIST})

target_include_directories(${TARGET_NAME}
	PUBLIC
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/RapidXML>
    $<INSTALL_INTERFACE:$<INSTALL_PREFIX>/${CMAKE_INSTALL_INCLUDEDIR}>
)

set_target_properties(${TARGET_NAME} PROPERTIES
    PUBLIC_HEADER "${HEADER_LIST}"
)

install(TARGETS ${TARGET_NAME}
	EXPORT ${TARGET_NAME}Targets
)

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    ${TARGET_NAME}ConfigVersion.cmake
    VERSION ${PACKAGE_VERSION}
    COMPATIBILITY ExactVersion
)

install(EXPORT ${TARGET_NAME}Targets
	FILE ${TARGET_NAME}Targets.cmake
	DESTINATION ${CMAKE_INSTALL_CMAKEDIR}/${TARGET_NAME}
)

configure_file(${PROJECT_SOURCE_DIR}/cmake/Config.cmake.in ${CMAKE_CURRENT_BINARY_DIR}/${TARGET_NAME}Config.cmake @ONLY)
install(FILES "${CMAKE_CURRENT_BINARY_DIR}/${TARGET_NAME}Config.cmake"
	"${CMAKE_CURRENT_BINARY_DIR}/${TARGET_NAME}ConfigVersion.cmake"
	DESTINATION ${CMAKE_INSTALL_CMAKEDIR}/${TARGET_NAME}
)