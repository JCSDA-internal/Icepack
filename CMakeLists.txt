# (C) Copyright 2018 UCAR.
#
# This software is licensed under the terms of the Apache Licence Version 2.0
# which can be obtained at http://www.apache.org/licenses/LICENSE-2.0.

################################################################################
# ICEPACK
################################################################################

cmake_minimum_required( VERSION 3.3.2 FATAL_ERROR )

project( icepack Fortran )

set( CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake;${CMAKE_MODULE_PATH} )

set( ECBUILD_DEFAULT_BUILD_TYPE Release )
set( ENABLE_OS_TESTS           OFF CACHE BOOL "Disable OS tests" FORCE )
set( ENABLE_LARGE_FILE_SUPPORT OFF CACHE BOOL "Disable testing of large file support" FORCE )
set( ENABLE_MPI ON CACHE BOOL "Compile with MPI" )

include( ecbuild_system NO_POLICY_SCOPE )

ecbuild_requires_macro_version( 2.7 )

################################################################################
# Project
################################################################################

ecbuild_declare_project()

ecbuild_enable_fortran( REQUIRED )

set( ICEPACK_LINKER_LANGUAGE Fortran )

################################################################################
# Dependencies
################################################################################

# LAPACK

# MPI
ecbuild_add_option( FEATURE MPI DEFAULT ON
                    DESCRIPTION "Support for MPI distributed parallelism"
                    REQUIRED_PACKAGES "MPI COMPONENTS Fortran" )
ecbuild_find_mpi( COMPONENTS Fortran REQUIRED )
ecbuild_include_mpi()
link_libraries(${MPI_Fortran_LIBRARIES})

# OpenMP
if( HAVE_OMP )
  ecbuild_enable_omp()
else()
  ecbuild_enable_ompstubs()
endif()

# NetCDF
set( NETCDF_F90 ON CACHE BOOL "Compile with Fortran NetCDF" )
find_package( NetCDF REQUIRED COMPONENTS Fortran )
include_directories( ${NETCDF_INCLUDE_DIRS} )

# fms

# cvmix

# gsw

# geoKdTree

# icepack_da_hooks

################################################################################
# Definitions
################################################################################

################################################################################
# Export package info
################################################################################

set( ICEPACK_INCLUDE_DIRS ${CMAKE_CURRENT_SOURCE_DIR}/configuration/driver
                       ${CMAKE_CURRENT_SOURCE_DIR}/columnphysics
                       ${CMAKE_Fortran_MODULE_DIRECTORY} )
set( ICEPACK_LIBRARIES icepack )

get_directory_property( ICEPACK_DEFINITIONS COMPILE_DEFINITIONS )

foreach( _tpl ${ICEPACK_TPLS} )
  string( TOUPPER ${_tpl} TPL )
  list( APPEND ICEPACK_EXTRA_DEFINITIONS   ${${TPL}_DEFINITIONS}  ${${TPL}_TPL_DEFINITIONS}  )
  list( APPEND ICEPACK_EXTRA_INCLUDE_DIRS  ${${TPL}_INCLUDE_DIRS} ${${TPL}_TPL_INCLUDE_DIRS} )
  list( APPEND ICEPACK_EXTRA_LIBRARIES     ${${TPL}_LIBRARIES}    ${${TPL}_TPL_LIBRARIES}    )
endforeach()

################################################################################
# Sources
################################################################################

include( icepack_compiler_flags )

include_directories( ${ICEPACK_INCLUDE_DIRS} ${ICEPACK_EXTRA_INCLUDE_DIRS} )

add_subdirectory( configuration )
add_subdirectory( columnphysics )

list( APPEND icepack_src_files
    ${configuration_files}
    ${columnphysics_files}
)

ecbuild_add_library( TARGET   icepack
                     SOURCES  ${icepack_src_files}
                     LIBS     ${LAPACK_LIBRARIES} ${NETCDF_LIBRARIES} 
                     INCLUDES 
                     INSTALL_HEADERS LISTED
                     LINKER_LANGUAGE ${ICEPACK_LINKER_LANGUAGE}
                    )

set ( icepack_exe_files
    configuration/driver/icedrv_MAIN.F90
)

ecbuild_add_executable( TARGET   icepack.x
                        SOURCES  ${icepack_exe_files}
                        LIBS     ${ICEPACK_LIBRARIES}
                        INCLUDES ${ICEPACK_INCLUDES}
                        LINKER_LANGUAGE ${ICEPACK_LINKER_LANGUAGE}
                       )

################################################################################
# Finalise configuration
################################################################################

# prepares a tar.gz of the sources and/or binaries
ecbuild_install_project( NAME icepack )

# print the summary of the configuration
ecbuild_print_summary()
