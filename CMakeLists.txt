cmake_minimum_required(VERSION 3.13)
project(joycond)

set(CMAKE_CXX_STANDARD 17)

# Generate compile_commands.json
set(CMAKE_EXPORT_COMPILE_COMMANDS 1)

find_package(PkgConfig REQUIRED)
pkg_check_modules(LIBEVDEV REQUIRED libevdev)
pkg_check_modules(LIBUDEV REQUIRED libudev)

add_executable(joycond "")
target_compile_options(joycond PRIVATE -Wall -Werror)
target_include_directories(joycond PRIVATE
    include/
    ${LIBEVDEV_INCLUDE_DIRS}
    ${LIBUDEV_INCLUDE_DIRS}
    )
target_link_libraries(joycond
    ${LIBEVDEV_LIBRARIES}
    ${LIBUDEV_LIBRARIES}
    )

add_subdirectory(src)

# Install to relative paths
install(TARGETS joycond DESTINATION bin
        PERMISSIONS OWNER_WRITE OWNER_READ OWNER_EXECUTE GROUP_READ GROUP_EXECUTE WORLD_READ WORLD_EXECUTE
        )
install(FILES udev/89-joycond.rules udev/72-joycond.rules DESTINATION udev/rules.d/
        PERMISSIONS OWNER_WRITE OWNER_READ GROUP_READ WORLD_READ 
        )
