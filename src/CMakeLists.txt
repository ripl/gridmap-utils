if(${CMAKE_SYSTEM_PROCESSOR} MATCHES arm*)
    add_compile_options(
        -march=armv8-a+fp+simd#+crypto+crc
        -std=gnu99)   
elseif(${CMAKE_SYSTEM_PROCESSOR} MATCHES amd64* OR ${CMAKE_SYSTEM_PROCESSOR} MATCHES x86_64*)
    add_compile_options(
        -std=gnu99
        -msse2)
elseif(${CMAKE_SYSTEM_PROCESSOR} MATCHES i686* OR ${CMAKE_SYSTEM_PROCESSOR} MATCHES x86)
    add_compile_options(
        -std=gnu99
        -msse2)
endif () 


file(GLOB c_files *.c)
file(GLOB h_files *.h)

include_directories(${GLIB2_INCLUDE_DIRS})

add_library(gridmap-utils SHARED ${c_files})

# Set the library API version. Increment every time the public API changes
set_target_properties(gridmap-utils PROPERTIES SOVERSION 1)

set(REQUIRED_LIBS bot2-core lcmtypes_gridmap geom-utils)

pods_use_pkg_config_packages(gridmap-utils ${REQUIRED_LIBS})

pods_install_headers(${h_files} DESTINATION gridmap_utils)

pods_install_libraries(gridmap-utils)

pods_install_pkg_config_file(gridmap-utils
    LIBS -lgridmap-utils
    REQUIRES ${REQUIRED_LIBS}
    VERSION 0.0.1)

pods_install_libraries(gridmap-utils)
