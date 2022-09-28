# set name of executable.
set(EXENAME hello_world)
# get source files to be preprocessed.
file(GLOB FYPPFILES RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} *.fypp)
# configurate preprocessing.
foreach(FYPPFILE ${FYPPFILES})
    string(REPLACE ".fypp" ".f90" SRCFILE ${FYPPFILE})
    add_custom_command(
        OUTPUT ${SRCFILE}
        DEPENDS ${FYPPFILE}
        COMMAND fypp ${CMAKE_CURRENT_SOURCE_DIR}/${FYPPFILE} ${SRCFILE} -Imacros.fypp $<$<CONFIG:Debug>:-DDEBUG=1> -DI_WANT_TO_GOODBYE=${I_WANT_TO_GOODBYE}
        WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    )
endforeach()
# get source files to be compiled.
string(REPLACE ".fypp" ".f90" SRCFILES "${FYPPFILES}")
# add executable target.
add_executable(${EXENAME} ${SRCFILES})
# set directory where fortran module files are generated.
set_target_properties(${EXENAME} PROPERTIES Fortran_MODULE_DIRECTORY ${MOD_DIR})
# add include directories for compiler when using libraries not found by cmake.
target_include_directories(${EXENAME} PRIVATE ${INCLUDE_PATH})
# add library directories for compiler when using libraries not found by cmake.
target_link_directories(${EXENAME} PRIVATE ${LIBRARY_PATH})
# link libraries.
target_link_libraries(${EXENAME} PUBLIC
    MPI::MPI_Fortran
    OpenMP::OpenMP_Fortran
)
# set compile optioins.
target_compile_options(${EXENAME} PRIVATE
    $<$<Fortran_COMPILER_ID:GNU>:-fopenmp -Wall -O3 -std=f2018>
    $<$<AND:$<Fortran_COMPILER_ID:GNU>,$<CONFIG:Debug>>:-fcheck=all -fbacktrace -g>
    $<$<Fortran_COMPILER_ID:Intel>:-qopenmp -warn all -O3 -std>
    $<$<AND:$<Fortran_COMPILER_ID:Intel>,$<CONFIG:Debug>>:-check all -traceback -g>
    $<$<Fortran_COMPILER_ID:Fujitsu>:-Kopenmp -Kfast -Kparallel -Nalloc_assign -check_std=08s>
    $<$<AND:$<Fortran_COMPILER_ID:Fujitsu>,$<CONFIG:Debug>>:-g>
)
# set linker options (normaly not needed)
target_link_options(${EXENAME} PRIVATE
    $<$<Fortran_COMPILER_ID:Fujitsu>:-Kparallel>
)
# configurate installation.
install(
    TARGETS ${EXENAME}
    RUNTIME DESTINATION bin
)
