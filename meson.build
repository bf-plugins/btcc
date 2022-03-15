# Bifrost wrapper file
project('btcc', 'cpp', 'cuda', version: '0.0.3',
         default_options : ['cpp_std=c++11'])
plugin_name = 'btcc'
cc = meson.get_compiler('cpp')
builddir = meson.project_build_root()

# CUDA setup
#cuda = import('unstable-cuda')
cuda_dep = dependency('cuda', version : '>=10', modules : ['cudart', 'nvrtc'])

# Dependency: bifrost
bf_dep = dependency('bifrost')
bifrost_src_path = '/home/dancpr/install/bifrost/src'

# List of directories to include when building plugin
bf_include = include_directories(
    bifrost_src_path, 
    bifrost_src_path / 'bifrost', 
    )
               
libtcc_proj = subproject('tcc')
libtcc_dep  = libtcc_proj.get_variable('subproject_dep')

# Generate shared object from .cu file
lib = library(plugin_name, 'src/btcc.cu',  
              version : '0.0.2', soversion : '0', 
              dependencies: [bf_dep, cuda_dep, libtcc_dep],
              include_directories: [bf_include])


r = run_command('cp', 'src/btcc_generated.py', builddir)
r = run_command('cp', 'src/__init__.py', builddir)