project('pybind11_protobuf', ['c','cpp'],
        license : 'TODO')

python3_config = find_program('python3-config')
python_include = run_command(python3_config, '--includes', check:false).stdout().strip().split(' ')[0].split('-I')[1]

sources = [
  'pybind11_protobuf/proto_cast_util.cc',
  'pybind11_protobuf/proto_utils.cc',
  'pybind11_protobuf/check_unknown_fields.cc'
]

pybind_subproj =  subproject('pybind11', required: true)
protoc_subproj = subproject('protobuf', 
                                  version: '>=3.21.6',
                                  required: true,
                                  default_options: ['warning_level=0', 'werror=false', 'default_library=shared'])

absl_subproj = subproject('abseil-cpp')

abs_deps = [absl_subproj.get_variable('absl_container_dep'), 
            absl_subproj.get_variable('absl_hash_dep'), 
            absl_subproj.get_variable('absl_strings_dep'),
            absl_subproj.get_variable('absl_synchronization_dep')]

deps = [protoc_subproj.get_variable('protoc_dep'), pybind_subproj.get_variable('pybind11_dep'), abs_deps]

pybind11_protobuf_native_proto_caster_lib = static_library('pybind11_protobuf_native_proto_caster', 
                                                            sources: sources,
                                                            include_directories: python_include,
                                                            dependencies: deps)

pybind11_protobuf_dep = declare_dependency(
                    include_directories: '.',
                    link_with: pybind11_protobuf_native_proto_caster_lib,
                    dependencies: abs_deps)

