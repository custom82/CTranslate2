set(CXXOPTS_INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/../third_party/cxxopts/include")

if (NOT IS_DIRECTORY "${CXXOPTS_INCLUDE_DIR}")
  find_path(CXXOPTS_INCLUDE_DIR
    NAMES cxxopts.hpp
    PATH_SUFFIXES cxxopts include
  )
endif()

if (NOT CXXOPTS_INCLUDE_DIR)
  message(FATAL_ERROR "The client dependency (cxxopts) is missing. "
    "Either initialize the submodule with \"git submodule update --init --recursive\" "
    "or install cxxopts and make sure CMake can find cxxopts.hpp.")
endif()

add_executable(translator
  translator.cc
  )
target_include_directories(translator
  PRIVATE ${CXXOPTS_INCLUDE_DIR}
  )
target_link_libraries(translator
  PRIVATE ${PROJECT_NAME}
)

set_target_properties(translator PROPERTIES OUTPUT_NAME ct2-translator)

install(
  TARGETS translator
  DESTINATION ${CMAKE_INSTALL_BINDIR}
  )
