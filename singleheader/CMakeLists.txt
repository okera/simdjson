#
# Amalgamation
#
# We should check whether bash is available here and avoid failures on systems where
# bash is unavailable.
if (NOT MSVC)
  ##
  # Important! The script amalgamate.sh is not generally executable. It
  # assumes that bash is at /bin/bash which may not be true.
  ###
  set(SINGLEHEADER_FILES simdjson.h simdjson.cpp amalgamate_demo.cpp README.md)
  add_custom_command(
    OUTPUT ${SINGLEHEADER_FILES}
    COMMAND ${CMAKE_COMMAND} -E env
      AMALGAMATE_SOURCE_PATH=${PROJECT_SOURCE_DIR}/src
      AMALGAMATE_INPUT_PATH=${PROJECT_SOURCE_DIR}/include
      AMALGAMATE_OUTPUT_PATH=${CMAKE_CURRENT_BINARY_DIR}
      bash ${CMAKE_CURRENT_SOURCE_DIR}/amalgamate.sh
    DEPENDS amalgamate.sh simdjson-source 
  )

  add_custom_target(amalgamate DEPENDS ${SINGLEHEADER_FILES})
  add_executable(amalgamate_demo amalgamate_demo.cpp)
  target_link_libraries(amalgamate_demo simdjson-include-source)
  add_test(amalgamate_demo amalgamate_demo ${EXAMPLE_JSON} ${EXAMPLE_NDJSON})
endif()