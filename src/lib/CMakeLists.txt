# Static libraries
# http://www.itk.org/pipermail/insight-users/2007-November/024141.html
# http://www.linux-magazin.de/Heft-Abo/Ausgaben/2007/02/Mal-ausspannen

set(LIB_SRC_BASE
  defs_sV.cpp
  defs_sV.hpp
  vector_sV.cpp
  shutter_sV.cpp
  intMatrix_sV.cpp
  interpolate_sV.cpp
  bezierTools_sV.cpp
  sourceField_sV.cpp
  libsvflow/src/flowField_sV.cpp
)

set(LIB_SRC_VIDEO
  defs_sV.h
  videoInfo_sV.cpp
  avconvInfo_sV.cpp
)

set(LIB_SRC_ARGS
  trivialArgsReader_sV.cpp
)

set(LIB_SRC_FLOWTOOLS
  flowTools_sV.cpp
  kernel_sV.cpp
)

set(LIB_SRC_FLOWVIS
  flowVisualization_sV.cpp
)


set(LIB_SRC_ENCODE
  macros_sV.h
  video_enc.h
  ffmpeg_writer.h
  ffmpeg_writer.cpp
  video_enc.cpp
)

if (OLD_FFMPEG)
set(LIB_SRC_ENCODE
	${LIB_SRC_ENCODE}
	  ffmpegEncode_sV.c
)
endif()

if (APPLE AND USE_QTKIT)
set(LIB_SRC_ENCODE
	${LIB_SRC_ENCODE}
	qtkit.h
	qtkit.mm
)
endif()

message(STATUS "FFMPEG libraries are at ${FFMPEG_LIBRARIES}")

include_directories(${FFMPEG_INCLUDE_PATHS})
include_directories(libsvflow)

add_library(sV  STATIC ${LIB_SRC_BASE})
target_link_libraries(sV  ${QT_LIBRARIES} sVflow)
qt5_use_modules(sV Core)
qt5_use_modules(sV Gui)

add_library(sVinfo  STATIC ${LIB_SRC_VIDEO})
qt5_use_modules(sVinfo Core)
qt5_use_modules(sVinfo Gui)
target_link_libraries(sVinfo  ${FFMPEG_LIBRARIES})

add_library(sVencode  STATIC ${LIB_SRC_ENCODE})
target_link_libraries(sVencode  ${FFMPEG_LIBRARIES} sVflow)
qt5_use_modules(sVencode Core)
qt5_use_modules(sVencode Gui)

add_library(sVflowtools  STATIC ${LIB_SRC_FLOWTOOLS})
target_link_libraries(sVflowtools sVflow)

add_library(sVvis  STATIC ${LIB_SRC_FLOWVIS})
qt5_use_modules(sVvis Core)
qt5_use_modules(sVvis Gui)
target_link_libraries(sVvis  sVflow ${QT_LIBRARIES})

