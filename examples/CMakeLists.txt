pkg_check_modules(CURLPP REQUIRED curlpp)

file(GLOB ExamplesFileList "*.cpp")
DisplayList("Using Example Files:" "${ExamplesFileList}")

# Create a meta target to create examples
add_custom_target(build_all_examples COMMENT A target that requires all the examples.)

message(STATUS "Creating build rules for Examples:")
foreach(Example ${ExamplesFileList})
  message(STATUS "\tCreating build rule for ${Example}")
  # Get the name without extension or directory
  get_filename_component(ExampleName ${Example} NAME_WE)
  # Define example executable
  add_executable(${ExampleName} ${Example})
  # Link example against curlpp
  target_link_libraries(${ExampleName} ${CURLPP_LDFLAGS})
  # make the meta target depend on this example.
  add_dependencies(build_all_examples ${ExampleName})
endforeach(Example ${ExamplesFileList})
