cmake_minimum_required(VERSION 3.8)
project(imgui CXX)

set(CMAKE_DEBUG_POSTFIX d)

set(IMGUI_INCLUDES_PUBLIC
    imgui.h
    imconfig.h
)

set(IMGUI_INCLUDES_PRIVATE
    imgui_internal.h
    imstb_textedit.h
)

set(IMGUI_SOURCES
    imgui.cpp
    imgui_demo.cpp
    imgui_draw.cpp
    imgui_widgets.cpp
)

add_library(${PROJECT_NAME}
    ${IMGUI_INCLUDES_PUBLIC}
    ${IMGUI_INCLUDES_PRIVATE}
    ${IMGUI_SOURCES}
)

file(GLOB IMGUI_BINDINGS ${CMAKE_CURRENT_SOURCE_DIR}/examples/imgui_impl_* )

target_include_directories(${PROJECT_NAME} PUBLIC $<INSTALL_INTERFACE:include>)

install(TARGETS ${PROJECT_NAME}
    EXPORT IMGUIExport
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
)

install(EXPORT IMGUIExport FILE ${PROJECT_NAME}Config.cmake NAMESPACE ${PROJECT_NAME}:: DESTINATION share/${PROJECT_NAME})

if(NOT IMGUI_SKIP_HEADERS)
    install(
        FILES ${IMGUI_INCLUDES_PUBLIC}
        DESTINATION include
    )
endif()

if(IMGUI_COPY_BINDINGS)
    install(
        FILES ${IMGUI_INCLUDES_PRIVATE}
        DESTINATION include
    )
    install(
        FILES ${IMGUI_BINDINGS}
        DESTINATION include/bindings
    )
endif()