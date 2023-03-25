cmake_minimum_required(VERSION 3.25)
project(wgpu-native-dist VERSION 0.0.1 DESCRIPTION "wgpu-native-dist" LANGUAGES CXX C)
set(CMAKE_SUPPRESS_REGENERATION true)

# Set a default build type if none was specified
set(default_build_type "Release")

# Set all configuration types
set(CMAKE_CONFIGURATION_TYPES Debug Release RelWithDebInfo)
if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
    message(STATUS "Setting build type to '${default_build_type}' as none was specified.")
    set(CMAKE_BUILD_TYPE "${default_build_type}" CACHE
        STRING "Choose the type of build." FORCE)
    # Set the possible values of build type for cmake-gui
    set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS
    "Release" "RelWithDebInfo")
endif()

set(platform "win32-x64")
set(lib_root_folder "${PROJECT_SOURCE_DIR}/lib/${platform}")
set(build_type_folder "release")

set(WGPU_NATIVE_BUILD_DLL TRUE)
set(lib_root_folder "${lib_root_folder}/shared/${build_type_folder}")
# TODO test for macos and linux
set(wgpu_native_sym_lib_name "${lib_root_folder}/wgpu_native.dll.lib")
set(wgpu_native_shared_lib_name "${lib_root_folder}/wgpu_native.dll")
# TODO macos and linux

add_library(wgpu_native SHARED IMPORTED GLOBAL)
add_library(wgpu::wgpu_native ALIAS wgpu_native)
set_property(TARGET wgpu_native PROPERTY
            IMPORTED_LOCATION ${wgpu_native_shared_lib_name})
set_property(TARGET wgpu_native PROPERTY
            IMPORTED_IMPLIB ${wgpu_native_sym_lib_name})
add_library(wgpu INTERFACE IMPORTED GLOBAL)
set_property(TARGET wgpu PROPERTY
    INTERFACE_LINK_LIBRARIES wgpu_native)
add_library(wgpu::wgpu ALIAS wgpu)
set_target_properties(wgpu PROPERTIES INTERFACE_INCLUDE_DIRECTORIES ${PROJECT_SOURCE_DIR}/include)
