
SET (html_HEADERS
  gnc-html-history.h
  gnc-html.h
  gnc-html-p.h
  gnc-html-factory.h
  gnc-html-extras.h
  gnc-html-webkit-p.h
  gnc-html-webkit.h
)

IF (BUILDING_FROM_VCS)
  SET (SWIG_GNC_HTML_C ${CMAKE_CURRENT_BINARY_DIR}/swig-gnc-html.c)
  GNC_ADD_SWIG_COMMAND (swig-gnc-html-c ${SWIG_GNC_HTML_C} ${CMAKE_CURRENT_SOURCE_DIR}/gnc-html.i "${gncmod_html_HEADERS}")
ELSE()
  SET (SWIG_GNC_HTML_C swig-gnc-html.c)
ENDIF()

SET (html_SOURCES
  gncmod-html.c
  gnc-html.c
  gnc-html-history.c
  gnc-html-factory.c
)

IF (WEBKIT1)
  LIST(APPEND html_HEADERS gnc-html-webkit1.h)
  LIST(APPEND html_SOURCES gnc-html-webkit1.c)
  SET(html_EXTRA_DIST gnc-html-webkit2.h gnc-html-webkit2.c)
ELSE ()
  LIST(APPEND html_HEADERS gnc-html-webkit2.h)
  LIST(APPEND html_SOURCES gnc-html-webkit2.c)
  SET(html_EXTRA_DIST gnc-html-webkit1.h gnc-html-webkit1.c)
ENDIF ()

SET_DIST_LIST(html_DIST CMakeLists.txt Makefile.am ${html_HEADERS} ${html_SOURCES} gnc-html.i ${html_EXTRA_DIST})

ADD_LIBRARY (gncmod-html
  ${html_SOURCES}
  ${SWIG_GNC_HTML_C}
  ${html_HEADERS}
)

TARGET_LINK_LIBRARIES(gncmod-html gncmod-engine gnc-module gncmod-gnome-utils ${WEBKIT_LDFLAGS} ${GUILE_LDFLAGS})

TARGET_COMPILE_DEFINITIONS(gncmod-html PRIVATE -DG_LOG_DOMAIN=\"gnc.html\")


TARGET_INCLUDE_DIRECTORIES (gncmod-html
PUBLIC
    ${GTK3_INCLUDE_DIRS}
    ${WEBKIT_INCLUDE_DIRS}
    ${CMAKE_CURRENT_SOURCE_DIR}
)

IF (APPLE)
  SET_TARGET_PROPERTIES (gncmod-html PROPERTIES INSTALL_NAME_DIR "${CMAKE_INSTALL_LIBDIR}/gnucash")
ENDIF()

INSTALL(TARGETS gncmod-html
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}/gnucash
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}/gnucash
  RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})

INSTALL(FILES ${html_HEADERS} DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/gnucash)
