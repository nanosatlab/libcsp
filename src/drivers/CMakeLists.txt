target_sources(csp PRIVATE usart/usart_kiss.c)

if(CSP_HAVE_LIBSOCKETCAN)
  add_library(driver_can OBJECT can/can_socketcan.c)
  target_include_directories(driver_can PUBLIC
    ${csp_inc}
    ${LIBSOCKETCAN_INCLUDE_DIRS})
  target_link_libraries(driver_can PUBLIC
    ${LIBSOCKETCAN_LIBRARIES})
  target_link_libraries(csp PUBLIC driver_can)
  if(BUILD_SHARED_LIBS)
    set_property(TARGET driver_can PROPERTY POSITION_INDEPENDENT_CODE ON)
  endif()
endif()

if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
  target_sources(csp PRIVATE usart/usart_linux.c)
endif()

if(HAVE_ZEPHYR_CAN)
  target_sources(csp PRIVATE can/can_zephyr.c)
endif()
