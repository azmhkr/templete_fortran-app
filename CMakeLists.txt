# require a minimum version of cmake.
cmake_minimum_required(VERSION 3.21)
# set project name.
project(hello_world Fortran)
# enable fortran.
enable_language(Fortran)
# specify directory where fortran module files are generated.
set(MOD_DIR ${CMAKE_CURRENT_BINARY_DIR}/mod_files/)
# set optional argument.
if(NOT I_WANT_TO_GOODBYE)
    set(I_WANT_TO_GOODBYE 0)
endif()
# find cmake packages i.e. external libraries built by cmake.
find_package(MPI REQUIRED)
find_package(OpenMP REQUIRED)
# add subdirectories
add_subdirectory(src)
