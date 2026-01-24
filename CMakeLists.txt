cmake_minimum_required(VERSION 3.20) # 3.20 is a good minimum, adjust as needed
project(Test CXX)

# Set C++ standard to 11
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Enable compile commands for better IDE support
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# Find wxWidgets package, specifying required components
find_package(wxWidgets REQUIRED gl core base OPTIONAL_COMPONENTS net)

# For modern CMake (>= 3.27), link the imported target directly
if(TARGET wxWidgets::wxWidgets)
    # Add your executable target
    add_executable(${PROJECT_NAME} main.cpp)

    # Link the target to the wxWidgets library
    target_link_libraries(${PROJECT_NAME} PRIVATE wxWidgets::wxWidgets)

else()
    # Fallback for older CMake versions (pre-3.27)
    include(${wxWidgets_USE_FILE})
    add_executable(${PROJECT_NAME} main.cpp)
    target_link_libraries(${PROJECT_NAME} PRIVATE ${wxWidgets_LIBRARIES})
endif()

# Platform-specific settings for proper application behavior
if(MSVC)
    # Use manifest from wx.rc for Visual Studio
    target_compile_flags(${PROJECT_NAME} PRIVATE "/DwxUSE_RC_MANIFEST")
    target_link_options(${PROJECT_NAME} PRIVATE "/MANIFEST:NO")
elseif(APPLE)
    # Basic settings for macOS bundles
    set_target_properties(${PROJECT_NAME} PROPERTIES
        MACOSX_BUNDLE_GUI_IDENTIFIER "org.wxwidgets.minimal"
        MACOSX_BUNDLE YES
    )
endif()
