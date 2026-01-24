# Minimal wxWidgets app

I just dicided to try wxWidgets just to use more C++ Thas one of my favorite coding languages, also researching in internet I found this but I couldn't find any good information using 
visual studio code so I'll try to express my procces as good as possible 

## My process 

- Install wxWidgets I recomend use the tutorial in the official webpage: https://docs.wxwidgets.org/3.3/overview_install.html
- (In my case I use macOS so I used the following comand ```brew install wxwidgets```
- I created a folder to create and build the project you can named as you want
- I created a cpp file named main.cpp and that's that I found in the official website if you want to copy it. https://wiki.wxwidgets.org/Hello_World
- Also I created a CMakeLists.txt with this code
```
  cmake_minimum_required(VERSION 3.20) # 3.20 is a good minimum, adjust as needed
project(MinimalWxExample CXX)

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
```
This one worked for me you can modify it if you want
- Also in my personal case I had to add the path of wxWidget on C/C++ Configurations on include path in my case were these
```
/usr/local/lib/wx/include/osx_cocoa-unicode-3.3
/usr/local/include/wx-3.3
```
you can try it if you want
- That's it!

## To build the project
In a bash or terminal just type
```
cmake .
```
and
```
make
```
You have to be on the path of your project 

## Screenshot of the app 
<img width="452" height="345" alt="imagen" src="https://github.com/user-attachments/assets/7708407b-26fc-4d52-9bb5-9db4ea69ea14" />
