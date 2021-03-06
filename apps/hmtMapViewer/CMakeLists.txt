INCLUDE(../../cmakemodules/AssureCMakeRootFile.cmake) # Avoid user mistake in CMake source directory

#-----------------------------------------------------------------
# CMake file for the MRPT application:  hmtMapViewer
#
#  Run with "cmake ." at the root directory
#
#  May 2008, Jose Luis Blanco <jlblanco@ctima.uma.es>
#-----------------------------------------------------------------
PROJECT(hmtMapViewer)

IF(wxWidgets_FOUND AND CMAKE_MRPT_HAS_OPENGL_GLUT)

	# ---------------------------------------------
	# TARGET:
	# ---------------------------------------------
	SET(SRCS
		hmtMapViewerApp.cpp hmtMapViewerApp.h
		hmtMapViewerMain.cpp hmtMapViewerMain.h
		)
	IF(WIN32)
#		SET(SRCS ${SRCS} resource.rc)
	ENDIF(WIN32)

	# Define the executable target:
	ADD_EXECUTABLE(hmtMapViewer WIN32 ${SRCS} ${MRPT_VERSION_RC_FILE})

	SET(TMP_TARGET_NAME "hmtMapViewer")

	

	# Add the required libraries for linking:
	INCLUDE_DIRECTORIES( ${wxWidgets_INCLUDE_DIRS} )
	TARGET_LINK_LIBRARIES(${TMP_TARGET_NAME} ${wxWidgets_LIBRARIES} )

	TARGET_LINK_LIBRARIES(${TMP_TARGET_NAME} ${MRPT_LINKER_LIBS})
	
	#   FabMap module for HMT-SLAM? (optional)
	IF(FabMap_FOUND)
		TARGET_LINK_LIBRARIES(${TMP_TARGET_NAME} ${FabMap_LIBRARIES})
	ENDIF(FabMap_FOUND)


	# Dependencies on MRPT libraries:
	#  Just mention the top-level dependency, the rest will be detected automatically, 
	#  and all the needed #include<> dirs added (see the script DeclareAppDependencies.cmake for further details)
	DeclareAppDependencies(${TMP_TARGET_NAME} mrpt-gui mrpt-slam mrpt-hmtslam)

	DeclareAppForInstall(${TMP_TARGET_NAME})


ELSE(wxWidgets_FOUND AND CMAKE_MRPT_HAS_OPENGL_GLUT)
	# ----------------------------------------------------------------------------------
    # For convenience. When we cannot continue, inform the user
	# ----------------------------------------------------------------------------------
    MESSAGE(STATUS "wxWidgets and/or GL/GLUT not found!: **This target will not be included in the Makefiles**")
ENDIF(wxWidgets_FOUND AND CMAKE_MRPT_HAS_OPENGL_GLUT)


