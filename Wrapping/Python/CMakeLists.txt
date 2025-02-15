cmake_minimum_required(VERSION 3.8 FATAL_ERROR)

# lowercase viennaLS is the python export
project("ViennaLSPython" VERSION ${VIENNALS_VERSION})

include(GNUInstallDirs)

message(STATUS "ViennaLS version: ${VIENNALS_VERSION}")

# include viennals
set(ViennaLS_DIR "${ViennaLS_BINARY_DIR}")

set(VIENNALS_PYTHON_SOURCE "pyWrap.cpp")
set(VIENNALS_PYTHON_MODULE_NAME "viennals")

set(CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE)

if(VIENNALS_BUILD_PYTHON_3)
  set(PYBIND11_PYTHON_VERSION
      3
      CACHE STRING "Python version")

  find_package(ViennaLS REQUIRED PATHS ${ViennaLS_DIR} NO_DEFAULT_PATH)
  list(PREPEND VIENNALS_INCLUDE_DIRS ${VIENNALS_BUILD_INCLUDE_DIRS})
  find_package(pybind11 REQUIRED PATHS ${pybind11_DIR})

  # make 3.6 python module define dimension for build
  set(VIENNALS_PYTHON_MODULE_NAME_2D "${VIENNALS_PYTHON_MODULE_NAME}2d")

  pybind11_add_module(${VIENNALS_PYTHON_MODULE_NAME_2D} ${VIENNALS_PYTHON_SOURCE})
  target_include_directories(${VIENNALS_PYTHON_MODULE_NAME_2D} PUBLIC ${VIENNALS_INCLUDE_DIRS})
  target_link_libraries(${VIENNALS_PYTHON_MODULE_NAME_2D} PRIVATE ${VIENNALS_PYTHON_LIBRARIES})
  # define compile dimension
  target_compile_definitions(${VIENNALS_PYTHON_MODULE_NAME_2D}
                             PRIVATE -DVIENNALS_PYTHON_DIMENSION=2)

  install(TARGETS ${VIENNALS_PYTHON_MODULE_NAME_2D} LIBRARY DESTINATION ${ViennaLS_BINARY_DIR})

  # define dimension for build
  set(VIENNALS_PYTHON_MODULE_NAME_3D "${VIENNALS_PYTHON_MODULE_NAME}3d")

  pybind11_add_module(${VIENNALS_PYTHON_MODULE_NAME_3D} ${VIENNALS_PYTHON_SOURCE})
  target_include_directories(${VIENNALS_PYTHON_MODULE_NAME_3D} PUBLIC ${VIENNALS_INCLUDE_DIRS})
  target_link_libraries(${VIENNALS_PYTHON_MODULE_NAME_3D} PRIVATE ${VIENNALS_PYTHON_LIBRARIES})
  # define compile dimension
  target_compile_definitions(${VIENNALS_PYTHON_MODULE_NAME_3D}
                             PRIVATE -DVIENNALS_PYTHON_DIMENSION=3)

  install(TARGETS ${VIENNALS_PYTHON_MODULE_NAME_3D} LIBRARY DESTINATION ${ViennaLS_BINARY_DIR})
endif()

if(VIENNALS_BUILD_PYTHON_2 AND NOT VIENNALS_BUILD_PYTHON_3)
  set(PYBIND11_PYTHON_VERSION
      2
      CACHE STRING "Python version")

  # disable register warning, as Python.h of Python 2 includes register variables
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-register")

  find_package(ViennaLS REQUIRED PATHS ${ViennaLS_DIR} NO_DEFAULT_PATH)
  list(PREPEND VIENNALS_INCLUDE_DIRS ${VIENNALS_BUILD_INCLUDE_DIRS})
  find_package(pybind11 REQUIRED PATHS ${pybind11_DIR})

  # make 2.7 python module
  set(VIENNALS_PYTHON_MODULE_NAME_2D "${VIENNALS_PYTHON_MODULE_NAME}2d")

  pybind11_add_module(${VIENNALS_PYTHON_MODULE_NAME_2D} ${VIENNALS_PYTHON_SOURCE})
  target_include_directories(${VIENNALS_PYTHON_MODULE_NAME_2D} PUBLIC ${VIENNALS_INCLUDE_DIRS})
  target_link_libraries(${VIENNALS_PYTHON_MODULE_NAME_2D} PRIVATE ${VIENNALS_PYTHON_LIBRARIES})
  # define compile dimension
  target_compile_definitions(${VIENNALS_PYTHON_MODULE_NAME_2D}
                             PRIVATE -DVIENNALS_PYTHON_DIMENSION=2)

  install(TARGETS ${VIENNALS_PYTHON_MODULE_NAME_2D} LIBRARY DESTINATION ${ViennaLS_BINARY_DIR})

  # define dimension for build
  set(VIENNALS_PYTHON_MODULE_NAME_3D "${VIENNALS_PYTHON_MODULE_NAME}3d")

  pybind11_add_module(${VIENNALS_PYTHON_MODULE_NAME_3D} ${VIENNALS_PYTHON_SOURCE})
  target_include_directories(${VIENNALS_PYTHON_MODULE_NAME_3D} PUBLIC ${VIENNALS_INCLUDE_DIRS})
  target_link_libraries(${VIENNALS_PYTHON_MODULE_NAME_3D} PRIVATE ${VIENNALS_PYTHON_LIBRARIES})
  # define compile dimension
  target_compile_definitions(${VIENNALS_PYTHON_MODULE_NAME_3D}
                             PRIVATE -DVIENNALS_PYTHON_DIMENSION=3)

  install(TARGETS ${VIENNALS_PYTHON_MODULE_NAME_3D} LIBRARY DESTINATION ${ViennaLS_BINARY_DIR})
endif()
