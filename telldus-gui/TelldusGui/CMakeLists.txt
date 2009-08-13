FIND_PACKAGE( Qt4 REQUIRED )
INCLUDE( ${QT_USE_FILE} )

if(COMMAND cmake_policy)
	cmake_policy(SET CMP0003 NEW)
endif(COMMAND cmake_policy)

######## Non configurable options  ########
SET( telldus-gui_SRCS
	telldusgui.cpp
	devicewidget.cpp
	devicemodel.cpp
	device.cpp
	editdevicedialog.cpp
	editgroupdialog.cpp
	vendordevicemodel.cpp
	vendordevicetreeitem.cpp
	devicesettingarctechselflearning.cpp
	devicesettingbrateck.cpp
	devicesettingikea.cpp
	devicesettingnexa.cpp
	devicesettingnexabell.cpp
	devicesettingrisingsun.cpp
	devicesettingsartano.cpp
	devicesettingupm.cpp
	devicesetting.cpp
	methodwidget.cpp
	deviceview.cpp
)

SET( telldus-gui_HDRS
	telldusgui.h
)

SET( telldus-gui_MOC_HDRS
	devicewidget.h
	devicemodel.h
	device.h
	editdevicedialog.h
	editgroupdialog.h
	vendordevicemodel.h
	vendordevicetreeitem.h
	devicesettingarctechselflearning.h
	devicesettingbrateck.h
	devicesettingikea.h
	devicesettingnexa.h
	devicesettingnexabell.h
	devicesettingrisingsun.h
	devicesettingsartano.h
	devicesettingupm.h
	devicesetting.h
	methodwidget.h
	deviceview.h
)

QT4_WRAP_CPP(  telldus-gui_MOC_SRCS  ${telldus-gui_MOC_HDRS} )
QT4_AUTOMOC (  ${telldus-gui_SRCS} )
QT4_ADD_RESOURCES  (telldus-gui_RSRCS     telldusgui.qrc         )

FIND_LIBRARY( TELLDUSCORE_LIBRARY telldus-core )

SET( telldus-gui_LIBRARIES
	${QT_LIBRARIES}
	${TELLDUSCORE_LIBRARY}
)

######## Configurable options for the platform  ########



######## Platforms-specific, non configurable  ########

IF (APPLE)
	#### Mac OS X ####
	SET( telldus-gui_TARGET	TelldusGui )
	ADD_DEFINITIONS(
		-D_MACOSX
	)
ELSEIF (WIN32)
	#### Windows ####
	SET( telldus-gui_TARGET	TelldusGui )
	ADD_DEFINITIONS(
		-D_WINDOWS
		-DTELLDUSGUI_EXPORTS
	)
	INCLUDE_DIRECTORIES(
		${TELLDUSCORE_INCLUDE_DIR}
	)
	SET( telldus-gui_LIBRARIES
		${telldus-gui_LIBRARIES}
		${TELLDUSCORE_LIBRARY}
	)
ELSE (APPLE)
	#### Linux ####
	SET( telldus-gui_TARGET	telldus-gui )
	ADD_DEFINITIONS(
		-D_LINUX
	)
ENDIF (APPLE)


######## Configuring  ########

ADD_LIBRARY(${telldus-gui_TARGET} SHARED
	${telldus-gui_SRCS}
	${telldus-gui_HDRS}
	${telldus-gui_MOC_SRCS}
	${telldus-gui_RSRCS}
)

TARGET_LINK_LIBRARIES( ${telldus-gui_TARGET}	${telldus-gui_LIBRARIES} )

SET_PROPERTY(TARGET ${telldus-gui_TARGET}
	PROPERTY FRAMEWORK TRUE
)
SET_PROPERTY(TARGET ${telldus-gui_TARGET}
	PROPERTY PUBLIC_HEADER ${telldus-gui_HDRS}
)

SET_PROPERTY(TARGET ${telldus-gui_TARGET}
	PROPERTY VERSION ${PACKAGE_VERSION}
)
SET_PROPERTY(TARGET ${telldus-gui_TARGET}
  PROPERTY SOVERSION ${PACKAGE_SOVERSION}
)

INSTALL(TARGETS ${telldus-gui_TARGET}
	DESTINATION lib
#	PUBLIC_HEADER       DESTINATION         ${HEADER_DIR}
	)