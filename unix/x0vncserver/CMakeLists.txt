include_directories(${X11_INCLUDE_DIR})
include_directories(${CMAKE_SOURCE_DIR}/unix/tx)
include_directories(${CMAKE_SOURCE_DIR}/unix/vncconfig)
include_directories(${CMAKE_SOURCE_DIR}/unix)
include_directories(${CMAKE_SOURCE_DIR}/common)

add_executable(x0vncserver
  buildtime.c
  Image.cxx
  x0vncserver.cxx
  ../vncconfig/QueryConnectDialog.cxx
)

target_link_libraries(x0vncserver tx rfb network rdr)

if(X11_FOUND AND X11_XTest_LIB)
  add_definitions(-DHAVE_XTEST)
  target_link_libraries(x0vncserver ${X11_XTest_LIB})
else()
  message(WARNING "No XTest extension.  x0vncserver will be view-only.")
endif()

if(X11_FOUND AND X11_Xdamage_LIB)
  add_definitions(-DHAVE_XDAMAGE)
  target_link_libraries(x0vncserver ${X11_Xdamage_LIB})
else()
  message(WARNING "No DAMAGE extension.  x0vncserver will have to use the slower polling method.")
endif()

target_link_libraries(x0vncserver ${X11_LIBRARIES})

install(TARGETS x0vncserver DESTINATION ${BIN_DIR})
install(FILES x0vncserver.man DESTINATION ${MAN_DIR}/man1 RENAME x0vncserver.1)
