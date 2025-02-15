esma_set_this (OVERRIDE MAPL.shared)

set (srcs
    hash.c
    hinterp.F
    MAPL_DirPath.F90
    MAPL_ErrorHandling.F90
    MAPL_Hash.F90
    MAPL_HeapMod.F90
    MAPL_KeywordEnforcer.F90
    MAPL_LoadBalance.F90
    MAPL_MinMax.F90
    MAPL_Range.F90
    MAPL_Sort.F90
    MAPL_Throw.F90
    AbstractCommSplitter.F90
    CommGroupDescription.F90
    SplitCommunicator.F90
    SimpleCommSplitter.F90
    sort.c
    MAPL_ExceptionHandling.F90
    String.F90
    MaplShared.F90
    TimeUtils.F90
    FileSystemUtilities.F90
    DSO_Utilities.F90
    MAPL_ISO8601_DateTime.F90
    MAPL_DateTime_Parsing.F90
    DownBit.F90
    ShaveMantissa.c
    MAPL_Sleep.F90
# Fortran submodules
    Interp/Interp.F90 Interp/Interp_implementation.F90
    Shmem/Shmem.F90   Shmem/Shmem_implementation.F90
    )

esma_add_library (${this} SRCS ${srcs} DEPENDENCIES MAPL.constants GFTL_SHARED::gftl-shared MPI::MPI_Fortran PFLOGGER::pflogger TYPE ${MAPL_LIBRARY_TYPE})

# CMake has an OpenMP issue with NAG Fortran: https://gitlab.kitware.com/cmake/cmake/-/issues/21280
if (NOT CMAKE_Fortran_COMPILER_ID MATCHES "NAG")
   target_link_libraries(${this} PRIVATE OpenMP::OpenMP_Fortran)
endif ()

target_include_directories (${this} PUBLIC $<BUILD_INTERFACE:${MAPL_SOURCE_DIR}/include>)

set_target_properties (${this} PROPERTIES Fortran_MODULE_DIRECTORY ${include_${this}})

target_compile_definitions(${this} PRIVATE SYSTEM_DSO_SUFFIX="${CMAKE_SHARED_LIBRARY_SUFFIX}")

if (PFUNIT_FOUND)
  add_subdirectory (tests)
endif ()

add_subdirectory(Constants)
#add_subdirectory(tests EXCLUDE_FROM_ALL)
