# Bifrost wrapper file
project('btcc', 'cpp', 'cuda', version: '0.0.4',
         default_options : ['cpp_std=c++11'])
plugin_name = 'btcc'
version = '0.0.4'
so_version = '0'

cc = meson.get_compiler('cpp')
builddir = meson.project_build_root()

# CUDA setup
#cuda = import('unstable-cuda')
cuda_dep = dependency('cuda', version : '>=10', modules : ['cudart', 'nvrtc'])

# Dependency: bifrost
bf_dep = dependency('bifrost')
bifrost_src_path = '/home/dancpr/install/bifrost/src'

# Dependency: python
python_sources = [
  'pysrc/__init__.py',
  'pysrc/btcc.py',
  'pysrc/btcc_generated.py'
]

py_mod = import('python')
py3 = py_mod.find_installation()
py3_dep = py3.dependency()
py3.install_sources(python_sources, 
                    subdir: plugin_name)

# List of directories to include when building plugin
bf_include = include_directories(
    bifrost_src_path, 
    bifrost_src_path / 'bifrost', 
    )
               
libtcc_proj = subproject('tcc')
libtcc_dep  = libtcc_proj.get_variable('subproject_dep')

# Generate shared object from .cu file
lib = library(plugin_name, 'src/btcc.cu',  
              version : version, soversion : so_version, 
              dependencies: [bf_dep, cuda_dep, libtcc_dep],
              include_directories: [bf_include],
              install: true)