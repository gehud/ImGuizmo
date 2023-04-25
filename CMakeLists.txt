project(ImGuizmo VERSION 1.0.0 LANGUAGES CXX)

file(
	GLOB_RECURSE 
	IMGIZMO_HEADER_FILES
	GraphEditor.h
	ImCurveEdit.h
	ImGradient.h
	ImGuizmo.h
	ImSequencer.h
	ImZoomSlider.h
)

file(
	GLOB_RECURSE 
	IMGIZMO_SOURCE_FILES
	GraphEditor.cpp
	ImCurveEdit.cpp
	ImGradient.cpp
	ImGuizmo.cpp
	ImSequencer.cpp
)

add_library(${PROJECT_NAME} STATIC
    ${IMGIZMO_HEADER_FILES}
    ${IMGIZMO_SOURCE_FILES}
)

add_library(ImGuizmo::ImGuizmo ALIAS ${PROJECT_NAME})

target_include_directories(${PROJECT_NAME} INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}>
	$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)

target_link_libraries(${PROJECT_NAME} ImGui)

include(GNUInstallDirs)

install(TARGETS ${PROJECT_NAME}
    EXPORT ImGuizmoTargets
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
	LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
	RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)
install(FILES ${IMGIZMO_HEADER_FILES} DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

install(
	EXPORT ImGuizmoTargets
	FILE Targets.cmake
	DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/ImGuizmo
	NAMESPACE ImGuizmo::
)

include(CMakePackageConfigHelpers)

configure_package_config_file( 
	${CMAKE_CURRENT_LIST_DIR}/cmake/Config.cmake.in
	${CMAKE_CURRENT_BINARY_DIR}/ImGuizmoConfig.cmake
	INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/ImGuizmo
	PATH_VARS
	CMAKE_INSTALL_LIBDIR
)

write_basic_package_version_file(
	${CMAKE_CURRENT_BINARY_DIR}/ImGuizmoConfigVersion.cmake
	VERSION ${PROJECT_VERSION}
	COMPATIBILITY SameMajorVersion
)

# Install Config and ConfigVersion files
install(
	FILES ${CMAKE_CURRENT_BINARY_DIR}/ImGuizmoConfig.cmake
		${CMAKE_CURRENT_BINARY_DIR}/ImGuizmoConfigVersion.cmake
	DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/ImGuizmo
)

set_target_properties(${PROJECT_NAME} PROPERTIES WINDOWS_EXPORT_ALL_SYMBOLS TRUE OUTPUT_NAME "imgizmo")