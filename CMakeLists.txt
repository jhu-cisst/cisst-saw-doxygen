#
# (C) Copyright 2015 Johns Hopkins University (JHU), All Rights Reserved.
#
# --- begin cisst license - do not edit ---
#
# This software is provided "as is" under an open source license, with
# no warranty.  The complete license can be found in license.txt and
# http://www.cisst.org/cisst/license.txt.
#
# --- end cisst license ---

cmake_minimum_required (VERSION 2.8)

if (NOT "${CMAKE_CURRENT_SOURCE_DIR}" STREQUAL "${CMAKE_CURRENT_BINARY_DIR}")
  message (FATAL_ERROR "You need to do an in-source build, i.e. run CMake in the source directory")
endif ()

# libraries we want to document
set (REQUIRED_CISST_LIBRARIES
     cisstCommon
     cisstVector
     cisstOSAbstraction
     cisstMultiTask
     cisstParameterTypes
     cisstRobot
     cisstNumerical
     cisstStereoVision
     cisstInteractive
     )

find_package (cisst REQUIRED ${REQUIRED_CISST_LIBRARIES})

if (cisst_FOUND_AS_REQUIRED)

  # load cisst configuration
  include (${CISST_USE_FILE})

  # components to look for
  set (SAW_COMPONENTS
       saw3Dconnexion
       sawATIForceSensor
       sawAtracsysFusionTrack
       sawBarrett
       sawCANBus
       sawCMUSphinx4
       sawClaronMicronTracker
       sawConstraintController
       sawControllers
       sawGLUTSimulator
       sawIntuitiveDaVinci
       sawIntuitiveResearchKit
       sawJR3ForceSensor
       sawKeyboard
       sawLoPoMoCo
       sawMedtronicStealthlink
       sawMicroScribeDigitizer
       sawNDITracker
       sawNovintFalcon
       sawOpenAL
       sawOpenDynamicsEngine
       sawOpenIGTLink
       sawOpenNI
       sawOpenSceneGraph
       sawRobotIO1394
       sawSartoriusScale
       sawSensablePhantom
       sawTextToSpeech
       sawTrajectories
       )

     # For main header files
     foreach (inc ${CISST_INCLUDE_DIR})
       set (ALL_INCLUDE_DIRS "${ALL_INCLUDE_DIRS} ${inc}")
     endforeach ()
     
     # build list of include directories for Doxygen
     foreach (lib ${REQUIRED_CISST_LIBRARIES})
       foreach (inc ${CISST_INCLUDE_DIR})
         set (ALL_INCLUDE_DIRS "${ALL_INCLUDE_DIRS} ${inc}/${lib}")
       endforeach ()
     endforeach ()

     # find sawKeyboard since it should always be compiled
     cisst_find_saw_component (sawKeyboard REQUIRED)

     if (sawKeyboard_FOUND)

       # use sawKeyboard and do a simple query replace assuming all components have the same directory structure
       foreach (comp ${SAW_COMPONENTS})
         foreach (inc ${sawKeyboard_INCLUDE_DIR})
           string (REPLACE "sawKeyboard" "${comp}" new_inc ${inc})
	   # append component name
	   set (new_inc "${new_inc}/${comp}")
	   if (EXISTS ${new_inc})
	     set (ALL_INCLUDE_DIRS "${ALL_INCLUDE_DIRS} ${new_inc}")
	   else ()
	     message ("The directory ${new_inc} doesn't exist")
	   endif ()
         endforeach ()
       endforeach ()

     else (sawKeyboard_FOUND)
       message (FATAL_ERROR "Can't find sawKeyboard.  We use the path to sawKeyboard to find all other SAW components so we really need it!")
     endif (sawKeyboard_FOUND)
     
     message ("The following directories will be used to generate the documentation: ${ALL_INCLUDE_DIRS}")
     
     configure_file ("${CMAKE_CURRENT_SOURCE_DIR}/Doxyfile.in"
                     "${CMAKE_CURRENT_SOURCE_DIR}/Doxyfile"
                      @ONLY@)

else (cisst_FOUND_AS_REQUIRED)
  message (FATAL_ERROR "You need cisst compiled with ${REQUIRED_CISST_LIBRARIES}")
endif (cisst_FOUND_AS_REQUIRED)
