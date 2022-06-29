# (C) Copyright 2018 UCAR.
#
# This software is licensed under the terms of the Apache Licence Version 2.0
# which can be obtained at http://www.apache.org/licenses/LICENSE-2.0.

################################################################################
# ICEPACK
################################################################################

cmake_minimum_required( VERSION 3.3.2 FATAL_ERROR )
project( icepack VERSION 1.2.0 LANGUAGES Fortran )

# ecbuild integration
find_package(ecbuild 3.3.2 REQUIRED)
include( ecbuild_system NO_POLICY_SCOPE )
ecbuild_declare_project()

list( APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)
include( icepack_compiler_flags )

################################################################################
# Dependencies
################################################################################

find_package(OpenMP COMPONENTS C Fortran)
find_package(MPI REQUIRED COMPONENTS C Fortran)
find_package(NetCDF REQUIRED COMPONENTS Fortran )

# OpenMP
#if( HAVE_OMP )
#  ecbuild_enable_omp()
#else()
#  ecbuild_enable_ompstubs()
#endif()

#set( ICEPACK_INCLUDE_DIRS  ${CMAKE_Fortran_MODULE_DIRECTORY} )
#set( ICEPACK_LIBRARIES icepack )

################################################################################
# Sources
################################################################################

set( ICEPACK_LINKER_LANGUAGE Fortran )

include_directories( ${ICEPACK_INCLUDE_DIRS})

add_subdirectory( configuration )
add_subdirectory( columnphysics )

list( APPEND icepack_src_files
    ${configuration_files}
    ${columnphysics_files}
)

#if (ECBUILD_INSTALL_FORTRAN_MODULES)
#  install (DIRECTORY ${CMAKE_Fortran_MODULE_DIRECTORY}/${CMAKE_CFG_INTDIR}
#    DESTINATION ${INSTALL_INCLUDE_DIR} )
#endif()
ecbuild_add_library( TARGET   icepack
                     SOURCES  ${icepack_src_files}
                     INSTALL_HEADERS LISTED
                     LINKER_LANGUAGE ${ICEPACK_LINKER_LANGUAGE}
                    )

# target_include_directories( icepack PRIVATE
#    ${CMAKE_Fortran_MODULE_DIRECTORY} )

target_link_libraries(icepack PUBLIC NetCDF::NetCDF_Fortran)
target_link_libraries(icepack PUBLIC MPI::MPI_Fortran)
target_link_libraries(icepack PUBLIC OpenMP::OpenMP_C OpenMP::OpenMP_Fortran)

# Fortran module output directory for build and install interfaces
set(MODULE_DIR module/${PROJECT_NAME}/${CMAKE_Fortran_COMPILER_ID}/${CMAKE_Fortran_COMPILER_VERSION})
set_target_properties(${PROJECT_NAME} PROPERTIES Fortran_MODULE_DIRECTORY ${CMAKE_BINARY_DIR}/${MODULE_DIR})
install(DIRECTORY ${CMAKE_BINARY_DIR}/${MODULE_DIR}/ DESTINATION ${MODULE_DIR})
target_include_directories(${PROJECT_NAME} INTERFACE
                            $<BUILD_INTERFACE:${CMAKE_BINARY_DIR}/${MODULE_DIR}>
                            $<INSTALL_INTERFACE:${MODULE_DIR}>)

# Installed .h include locations
target_include_directories(icepack PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/configuration>
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/columnphysics>
  $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)

################################################################################
# Executable
################################################################################
#set ( icepack_exe_files
#    configuration/driver/icedrv_MAIN.F90
#)
#
#ecbuild_add_executable( TARGET   icepack.x
#                        SOURCES  ${icepack_exe_files}
#                        LIBS     ${ICEPACK_LIBRARIES}
#                        INCLUDES ${ICEPACK_INCLUDES}
#                        LINKER_LANGUAGE ${ICEPACK_LINKER_LANGUAGE}
#                       )
#
################################################################################
# Finalise configuration
################################################################################

ecbuild_install_project( NAME icepack )
ecbuild_print_summary()
