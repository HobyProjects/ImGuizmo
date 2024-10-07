cmake_minimum_required(VERSION 3.15)
project(imguizmo C CXX)

set(IMGIZMOS_LIBRARY imguizmo)

set(
	IMGIZMOS_HEADERS
		imgui/imgui.h
		imgui/imconfig.h
		imgui/imgui_internal.h
		imgui/imstb_rectpack.h
		imgui/imstb_textedit.h
		imgui/imstb_truetype.h

		ImGuizmo.h
		ImSequencer.h
		ImZoomSlider.h
		ImCurveEdit.h
		GraphEditor.h
		ImGradient.h
)

set(
	IMGIZMOS_SOURCES
		imgui/imgui.cpp
		imgui/imgui_demo.cpp
		imgui/imgui_draw.cpp
		imgui/imgui_tables.cpp
		imgui/imgui_widgets.cpp
		
		ImGuizmo.cpp
		ImSequencer.cpp
		ImCurveEdit.cpp
		GraphEditor.cpp
		ImGradient.cpp
)

add_library(${IMGIZMOS_LIBRARY} STATIC ${IMGIZMOS_SOURCES} ${IMGIZMOS_HEADERS})
target_include_directories(
	${IMGIZMOS_LIBRARY} 
		PUBLIC 
			${CMAKE_CURRENT_SOURCE_DIR}/imgui
)
add_library(imguizmo::imguizmo ALIAS ${IMGIZMOS_LIBRARY})

set_target_properties(
	${IMGIZMOS_LIBRARY} PROPERTIES
	CXX_STANDARD 20
	CXX_STANDARD_REQUIRED ON
	CXX_EXTENSIONS OFF
	C_STANDARD 17
	C_STANDARD_REQUIRED ON
	C_EXTENSIONS OFF
)

include(GNUInstallDirs)

target_include_directories(
	${IMGIZMOS_LIBRARY} PUBLIC
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
	$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)

install(
	TARGETS ${IMGIZMOS_LIBRARY}
	EXPORT "${IMGIZMOS_LIBRARY}Targets"
	LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
	ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
	RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
	INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR} 
)

install(
	FILES ${IMGIZMOS_HEADERS}
	DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/${IMGIZMOS_LIBRARY}
)

install(
	EXPORT "${IMGIZMOS_LIBRARY}Targets"
	FILE "${IMGIZMOS_LIBRARY}Targets.cmake"
	NAMESPACE imguizmo::
	DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${IMGIZMOS_LIBRARY}
)

include(CMakePackageConfigHelpers)

configure_package_config_file(
	"${CMAKE_CURRENT_LIST_DIR}/config/${IMGIZMOS_LIBRARY}Config.cmake.in"
	"${CMAKE_CURRENT_BINARY_DIR}/${IMGIZMOS_LIBRARY}Config.cmake"
	INSTALL_DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/${IMGIZMOS_LIBRARY}"
)

configure_package_config_file(
	"${CMAKE_CURRENT_LIST_DIR}/config/${IMGIZMOS_LIBRARY}.pc.in"
	"${CMAKE_CURRENT_BINARY_DIR}/${IMGIZMOS_LIBRARY}.pc"
	INSTALL_DESTINATION "${CMAKE_INSTALL_LIBDIR}/pkgconfig"
)

install(
	FILES "${CMAKE_CURRENT_BINARY_DIR}/${IMGIZMOS_LIBRARY}Config.cmake"
	DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/${IMGIZMOS_LIBRARY}"
)

install(
	FILES "${CMAKE_CURRENT_BINARY_DIR}/${IMGIZMOS_LIBRARY}.pc"
	DESTINATION "${CMAKE_INSTALL_LIBDIR}/pkgconfig"
)