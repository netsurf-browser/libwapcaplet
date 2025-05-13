project('wapcaplet', 'c',
  version: '0.4.3',
  default_options: ['c_std=c99', 'warning_level=2'])

cc = meson.get_compiler('c')

# Compiler flags
cflags = ['-D_BSD_SOURCE', '-D_DEFAULT_SOURCE', '-DSTMTEXPR']
if cc.get_id() != 'gcc' or cc.version().split('.')[0].to_int() >= 3
  cflags += ['-Wstrict-prototypes', '-Wmissing-prototypes', '-Wmissing-declarations', '-Wnested-externs']
endif
if host_machine.system() != 'haiku'
  cflags += ['-Werror']
endif

# Source files
wapcaplet_sources = files(
  'src/libwapcaplet.c'
)

# Include directories
inc = include_directories('include')

# Build the static library
wapcaplet_lib = static_library('wapcaplet',
  sources: wapcaplet_sources,
  include_directories: inc,
  c_args: cflags,
  install: true
)

# Install headers
install_headers('include/libwapcaplet/libwapcaplet.h',
  subdir: 'libwapcaplet'
)

# Pkg-config file
pkg = import('pkgconfig')
pkg.generate(wapcaplet_lib,
  name: 'libwapcaplet',
  description: 'A string internment library',
  version: '0.4.3',
  subdirs: ''  # Set to empty to use ${includedir} instead of ${includedir}/libwapcaplet
)

# Tests
if get_option('tests')
  check_dep = dependency('check', required: false)
  if check_dep.found()
    test_exe = executable('wapcaplet_test',
      sources: ['test/testmain.c', 'test/basictests.c'],
      include_directories: inc,
      link_with: wapcaplet_lib,
      c_args: cflags,
      dependencies: check_dep
    )
    test('wapcaplet_tests', test_exe)
  endif
endif