#----------------------------------------------------------------------------

# Setup the project
#
cmake_minimum_required(VERSION 2.6 FATAL_ERROR)
project(g4mc)

enable_language (Fortran)


#ROOT path
set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake/modules")
find_package(ROOT REQUIRED)
#find_package( PythonInterp REQUIRED )
#find_package(PythonLibs 2.7.5 EXACT REQUIRED)
#include_directories(${PYTHON_INCLUDE_DIR})

include_directories(${PROJECT_SOURCE_DIR}/include ${ROOT_INCLUDE_DIRS}
    )


#----------------------------------------------------------------------------
# Find Geant4 package, activating all available UI and Vis drivers by default
# You can set WITH_GEANT4_UIVIS to OFF via the command line or ccmake/cmake-gui
# to build a batch mode only executable
#
option(WITH_GEANT4_UIVIS "Build example with Geant4 UI and Vis drivers" ON)
if(WITH_GEANT4_UIVIS)
  find_package(Geant4 REQUIRED ui_all vis_all)
else()
  find_package(Geant4 REQUIRED)
endif()

#----------------------------------------------------------------------------
# Setup Geant4 include directories and compile definitions
# Setup include directory for this project
#
include(${Geant4_USE_FILE})


#debugging symbols, complains
set(CMAKE_CXX_FLAGS "-g -Wall -pg")

#----------------------------------------------------------------------------
# Generate dictionaries as well (so we can write objects to ROOT files

include_directories(${PROJECT_SOURCE_DIR}/include ${ROOT_INCLUDE_DIRS})


#----------------------------------------------------------------------------
# Locate sources and headers for this project
# NB: headers are included so they will show up in IDEs
#
file(GLOB sources ${PROJECT_SOURCE_DIR}/src/*.f
	  ${PROJECT_SOURCE_DIR}/src/*.cc
	  	  
)
file(GLOB headers ${PROJECT_SOURCE_DIR}/include/*.hh)


#----------------------------------------------------------------------------
# Add the executable, and link it to the Geant4 libraries
#
add_executable(G4MC G4MC.cc ${sources} ${headers})
#target_link_libraries(G4MC ${Geant4_LIBRARIES} ${ROOT_LIBRARIES} HRSTransport XSModel ${PYTHON_LIBRARIES})
target_link_libraries(G4MC ${Geant4_LIBRARIES} ${ROOT_LIBRARIES})


#g++ -I/usr/include/python2.7 -lpython2.7 ./fringe.C -o fringe
#----------------------------------------------------------------------------
# For internal Geant4 use - but has no effect if you build this
# example standalone
#
#add_custom_target(g4mc DEPENDS G4MC)

#----------------------------------------------------------------------------
# Install the executable to 'bin' directory under CMAKE_INSTALL_PREFIX
#
install(TARGETS G4MC DESTINATION bin)


