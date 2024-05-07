# ================================ #
# === Create build targets ======= #
# ================================ #
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)

file(GLOB SOURCES "./*.cpp")

# Add icons
include(${CMAKE_SOURCE_DIR}/src/AddIconToBinary.cmake)
AddIconToBinary(SOURCE_FILES ICONS ${CMAKE_SOURCE_DIR}/src/assets/icon.ico ${CMAKE_SOURCE_DIR}/src/assets/icon.icns)

message(BUDLING_FOR " Building for Windows")
add_executable(${TARGET_NAME} ${SOURCES} ${SOURCE_FILES} ${HEADERS})

target_link_libraries(${TARGET_NAME} PRIVATE 
    cryptopp::cryptopp 
    Qt6::Core 
    Qt6::Gui 
    Qt6::Widgets 
    Qt6::Platform 
) 

# If Windows, add  Qt6::QWindowsIntegrationPlugin to the target_link_libraries
if (MSVC)
    target_link_libraries(${TARGET_NAME} PRIVATE Qt6::QWindowsIntegrationPlugin)
endif()

install(TARGETS ${TARGET_NAME} DESTINATION ${CMAKE_INSTALL_BINDIR})

# Copy over the assets directory
add_custom_command(TARGET ${TARGET_NAME} POST_BUILD COMMAND ${CMAKE_COMMAND} -E copy_directory ${CMAKE_CURRENT_SOURCE_DIR}/assets $<TARGET_FILE_DIR:${TARGET_NAME}>/assets)

# ================================ #
# === Set up test  environment === #
# ================================ #
if (BUILD_TESTS)
    message(STATUS "Building tests")
    message(PATH "${CMAKE_SOURCE_DIR}/src/tests")
    add_subdirectory(${CMAKE_SOURCE_DIR}/src/tests)
endif ()