#SET(IGTL_UTIL_PROTOCOL_VERSION "1")
#SET(IGTL_UTIL_PROTOCOL_VERSION ${OpenIGTLink_PROTOCOL_VERSION})

if(NOT DEFINED igtlutil_BUILD_SHARED_LIBS)
  set(igtlutil_BUILD_SHARED_LIBS ${OpenIGTLink_BUILD_SHARED_LIBS})
endif()

if(NOT DEFINED igtlutil_INSTALL_DEVELOPMENT)
  set(igtlutil_INSTALL_DEVELOPMENT ${OpenIGTLink_INSTALL_DEVELOPMENT})
endif()

if(NOT DEFINED igtlutil_BUILD_GENERATE_PIC)
  set(igtlutil_BUILD_GENERATE_PIC ${OpenIGTLink_BUILD_GENERATE_PIC})
  set(IGTLSTATIC 1)
endif()

if(NOT DEFINED igtlutil_INSTALL_BIN_DIR)
  set(igtlutil_INSTALL_BIN_DIR ${OpenIGTLink_INSTALL_BIN_DIR})
endif()

if(NOT DEFINED igtlutil_INSTALL_LIB_DIR)
  set(igtlutil_INSTALL_LIB_DIR ${OpenIGTLink_INSTALL_LIB_DIR})
endif()

SET(igtlutil_HEADS
  igtl_header.h
  igtl_image.h
  igtl_position.h
  igtl_transform.h
  igtl_types.h
  igtl_util.h
  igtl_capability.h
  )

SET(igtlutil_SOURCES
  igtl_header.c
  igtl_image.c
  igtl_position.c
  igtl_transform.c
  igtl_util.c
  igtl_capability.c
  )

if (${OpenIGTLink_PROTOCOL_VERSION} GREATER "1")
  SET(igtlutil_HEADS
    ${igtlutil_HEADS}
    igtl_colortable.h
    igtl_imgmeta.h
    igtl_lbmeta.h
    igtl_point.h
    igtl_tdata.h
    igtl_qtdata.h
    igtl_trajectory.h
    igtl_unit.h
    igtl_sensor.h
    igtl_string.h
    igtl_ndarray.h
    igtl_bind.h
    igtl_qtrans.h
    igtl_polydata.h
    )
  SET(igtlutil_SOURCES
    ${igtlutil_SOURCES}
    igtl_colortable.c
    igtl_imgmeta.c
    igtl_lbmeta.c
    igtl_point.c
    igtl_tdata.c
    igtl_qtdata.c
    igtl_trajectory.c
    igtl_unit.c
    igtl_sensor.c
    igtl_string.c
    igtl_ndarray.c
    igtl_bind.c
    igtl_qtrans.c
    igtl_polydata.c
    )
endif (${OpenIGTLink_PROTOCOL_VERSION} GREATER "1")

if (${OpenIGTLink_PROTOCOL_VERSION} GREATER "2")
  SET(igtlutil_HEADS
    ${igtlutil_HEADS}
    igtl_command.h
    )
  SET(igtlutil_SOURCES
    ${igtlutil_SOURCES}
    igtl_command.c
    )
endif (${OpenIGTLink_PROTOCOL_VERSION} GREATER "2")

IF( MSVC OR ${CMAKE_GENERATOR} MATCHES "Xcode")
  ADD_LIBRARY(igtlutil
    ${igtlutil_SOURCES}
    ${igtlutil_HEADS}
    )
ELSE()
  ADD_LIBRARY(igtlutil
    ${igtlutil_SOURCES}
    )
ENDIF()

IF(MSVC)
  target_compile_options(igtlutil PRIVATE /MP)
ENDIF()

SET_TARGET_PROPERTIES(igtlutil PROPERTIES
  VERSION ${OpenIGTLink_VERSION_MAJOR}.${OpenIGTLink_VERSION_MINOR}.${OpenIGTLink_VERSION_PATCH}
  SOVERSION ${OpenIGTLink_VERSION_MAJOR}
)

find_library(igtlutil_HAVE_LIBM m)
if(igtlutil_HAVE_LIBM)
  target_link_libraries(igtlutil m)
endif()

if(igtlutil_BUILD_SHARED_LIBS OR (NOT igtlutil_BUILD_SHARED_LIBS AND igtlutil_INSTALL_DEVELOPMENT))
  install(TARGETS igtlutil EXPORT igtlutil
    RUNTIME DESTINATION ${igtlutil_INSTALL_BIN_DIR} COMPONENT RuntimeLibraries
    LIBRARY DESTINATION ${igtlutil_INSTALL_LIB_DIR} COMPONENT RuntimeLibraries
    ARCHIVE DESTINATION ${igtlutil_INSTALL_LIB_DIR} COMPONENT Development)
endif()
