esma_set_this (OVERRIDE MAPL.geom)


#set(srcs
#  EsmfRegridder.F90
#  FieldBLAS.F90
#  FieldBLAS_IntrinsicFunctions.F90
#  GeomManager.F90
#  MaplGeom.F90
#  Regridder.F90
#  StateSupplement.F90
#  )
set(srcs
  geom.F90
  FieldBLAS.F90
  FieldPointerUtilities.F90
  FieldUtilities.F90
  FieldUnaryFunctions.F90
  FieldBinaryOperations.F90
  )
# Workaround for strict NAG Fortran with ESMF implicit interface for private state.
#set_property( SOURCE InnerMetaComponent.F90 OuterMetaComponent.F90
#  PROPERTY COMPILE_FLAGS ${MISMATCH})

list (APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}")

if (BUILD_WITH_PFLOGGER)
  find_package (PFLOGGER REQUIRED)
endif ()

esma_add_library(${this}
  SRCS ${srcs}
  DEPENDENCIES MAPL.shared PFLOGGER::pflogger
  TYPE ${MAPL_LIBRARY_TYPE}
  )

#add_subdirectory(specs)
#add_subdirectory(registry)
#add_subdirectory(connection_pt)

target_include_directories (${this} PUBLIC
  $<BUILD_INTERFACE:${MAPL_SOURCE_DIR}/include>)
#target_link_libraries (${this} PUBLIC esmf NetCDF::NetCDF_Fortran)
target_link_libraries (${this} PUBLIC esmf)

if (PFUNIT_FOUND)
  # Turning off until test with GNU can be fixed
  add_subdirectory(tests EXCLUDE_FROM_ALL)
endif ()

