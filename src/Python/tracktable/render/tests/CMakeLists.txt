#
# Copyright (c) 2014-2023 National Technology and Engineering
# Solutions of Sandia, LLC. Under the terms of Contract DE-NA0003525
# with National Technology and Engineering Solutions of Sandia, LLC,
# the U.S. Government retains certain rights in this software.
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions
# are met:
#
# 1. Redistributions of source code must retain the above copyright
# notice, this list of conditions and the following disclaimer.
#
# 2. Redistributions in binary form must reproduce the above copyright
# notice, this list of conditions and the following disclaimer in the
# documentation and/or other materials provided with the distribution.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
# "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
# LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS
# FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE
# COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT,
# INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING,
# BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
# LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER
# CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT
# LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN
# ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
# POSSIBILITY OF SUCH DAMAGE.


# This is src/tracktable/render/tests/CMakeLists.txt
#
# Here we list the Python tests that we need to run to make sure that
# our rendering code is working.  We test the output of static images
# to make sure they match ground truth.  For HTML-based tests with Folium
# we compare the generated HTML+JavaScript against saved ground truth.

include(PythonTest)
include(FindPythonModule)

find_python_module(cartopy CARTOPY_MODULE_STATUS CARTOPY_MODULE_LOCATION)

find_package_handle_standard_args(PY_cartopy_module DEFAULT_MSG PY_CARTOPY_MODULE)
# message("PY_CARTOPY_MODULE ${PY_CARTOPY_MODULE}")

add_python_test(P_Folium_Proxy_Import tracktable.render.tests.test_folium_proxy_import)
add_python_test(P_Folium_Proxy_Disabled tracktable.render.tests.test_folium_proxy_disabled)
add_python_test(P_Folium_Fallback_Import tracktable.render.tests.test_folium_proxy_import_fallback)
add_python_test(P_Folium_Proxy_Import_Dotted_Package tracktable.render.tests.test_folium_proxy_import_dotted_package)


if (PY_CARTOPY_MODULE)
  # TODO (mjfadem): Get these tests up and running once we chase the gremlins out of the existing mapmaker tests

  # add_python_test(P_Mapmaker_City
  #  tracktable.render.tests.test_mapmaker_city "${Tracktable_SOURCE_DIR}/internal_test_data/GroundTruth" "${Tracktable_BINARY_DIR}/TestOutput/"
  # )

  # add_python_test(P_Mapmaker_Airport
  #  tracktable.render.tests.test_mapmaker_airport "${Tracktable_SOURCE_DIR}/internal_test_data/GroundTruth" "${Tracktable_BINARY_DIR}/TestOutput/"
  # )

  # add_python_test(P_Mapmaker_Port
  #  tracktable.render.tests.test_mapmaker_port "${Tracktable_SOURCE_DIR}/internal_test_data/GroundTruth" "${Tracktable_BINARY_DIR}/TestOutput/"
  # )

  add_python_test(P_Mapmaker_CONUS
    tracktable.render.tests.test_mapmaker_conus "${Tracktable_DATA_DIR}/internal_test_data/GroundTruth" "${Tracktable_BINARY_DIR}/TestOutput/"
  )

  #add_python_test(P_Mapmaker_NorthAmerica
  #  tracktable.render.tests.test_mapmaker_north_america "${Tracktable_DATA_DIR}/internal_test_data/GroundTruth" "${Tracktable_BINARY_DIR}/TestOutput/"
  #)

  add_python_test(P_Mapmaker_Custom_Bounding_Box_Object
    tracktable.render.tests.test_mapmaker_custom_bbox_object "${Tracktable_DATA_DIR}/internal_test_data/GroundTruth" "${Tracktable_BINARY_DIR}/TestOutput"
  )

  add_python_test(P_Mapmaker_Custom_Bounding_Box_Floats
    tracktable.render.tests.test_mapmaker_custom_bbox_float "${Tracktable_DATA_DIR}/internal_test_data/GroundTruth" "${Tracktable_BINARY_DIR}/TestOutput"
  )

  add_python_test(P_Mapmaker_Europe
    tracktable.render.tests.test_mapmaker_europe "${Tracktable_DATA_DIR}/internal_test_data/GroundTruth" "${Tracktable_BINARY_DIR}/TestOutput/"
  )

  add_python_test(P_Render_Trajectories
    tracktable.render.tests.test_render_trajectories "${Tracktable_DATA_DIR}/internal_test_data/GroundTruth" "${Tracktable_BINARY_DIR}/TestOutput/" "${Tracktable_DATA_DIR}/internal_test_data/Points"
  )

else (PY_CARTOPY_MODULE)
  message(STATUS "Cartopy module not found. Rendering tests will not be executed and rendering module will not execute with current python implementation.")
  message(STATUS "Rendering modules will be installed, but you must add cartopy to your current environment or change to a python environment with cartopy for rendering to work.")
endif(PY_CARTOPY_MODULE)
