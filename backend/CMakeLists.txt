# (c) 2008 by BogDan Vatra < bogdan@licentia.eu >

project(zint)

find_package(PNG)
find_package(Qr)

set(zint_COMMON_SRCS common.c png.c library.c ps.c large.c reedsol.c)
set(zint_ONEDIM_SRCS code.c code128.c 2of5.c upcean.c telepen.c medical.c plessey.c rss.c)
set(zint_POSTAL_SRCS postal.c auspost.c imail.c)
set(zint_TWODIM_SRCS code16k.c blockf.c dmatrix.c dm200.c pdf417.c qr.c maxicode.c composite.c aztec.c)
set(zint_SRCS ${zint_COMMON_SRCS} ${zint_ONEDIM_SRCS} ${zint_POSTAL_SRCS} ${zint_TWODIM_SRCS} )

if(PNG_FOUND)
    include_directories( ${PNG_INCLUDES} )
else(PNG_FOUND)
    add_definitions (-DNO_PNG)
endif(PNG_FOUND)

if(QR_FOUND)
    include_directories( ${QR_INCLUDES} )
else(QR_FOUND)
    add_definitions (-DNO_QR)
endif(QR_FOUND)

add_library(zint SHARED ${zint_SRCS})

set_target_properties(zint PROPERTIES 	VERSION ${ZINT_VERSION}
					SOVERSION ${ZINT_VERSION})

target_link_libraries(zint ${PNG_LIBRARIES} ${QR_LIBRARIES} )

install(TARGETS zint LIBRARY DESTINATION lib)
install(FILES zint.h DESTINATION include COMPONENT Devel)