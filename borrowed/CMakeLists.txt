ADD_SUBDIRECTORY(libc)
ADD_SUBDIRECTORY(goffice)
if (WITH_AQBANKING)
  ADD_SUBDIRECTORY(gwengui-gtk3)
endif()
SET_LOCAL_DIST(borrowed_DIST_local CMakeLists.txt Makefile.am README)
SET(borrowed_DIST ${borrowed_DIST_local} ${libc_DIST} ${goffice_DIST} ${gwengui_gtk3_DIST} PARENT_SCOPE)
