# adapted from https://github.com/mesonbuild/lua/tree/5.3.6
project(
  'lua-5.0', 'c',
  license: 'MIT',
  meson_version: '>=0.49.2',
  version: '5.0.2',
)
# Override the c_std and warning_level of any superproject.
project_options = ['c_std=c99', 'warning_level=2']
cc = meson.get_compiler('c')

# Skip bogus warning.
if cc.get_id() == 'clang'
  add_project_arguments('-Wno-string-plus-int', language: 'c')
elif cc.get_id() == 'gcc'
  add_project_arguments('-Wno-stringop-overflow', language: 'c')
endif

# Platform-specific defines.
is_posix = host_machine.system() not in ['windows', 'emscripten', 'android']
if is_posix
  add_project_arguments('-DLUA_USE_POSIX', language: 'c')
endif

add_project_arguments('-DLUA_USER_H="../etc/luser_number.h"', language: 'c')
add_project_arguments('-DUSE_INT', language: 'c')

# Library dependencies.
lua_lib_deps = [cc.find_library('m', required: false)]

inc_inc = include_directories('include')

# Targets.
inc_src = include_directories('src')
lua = static_library('lua',
  'src/lgc.c',
  'src/lmem.c',
  'src/ltests.c',
  'src/ldump.c',
  'src/ltm.c',
  'src/ldo.c',
  'src/ldebug.c',
  'src/lobject.c',
  'src/lstring.c',
  'src/lzio.c',
  'src/lopcodes.c',
  'src/lcode.c',
  'src/lstate.c',
  'src/lparser.c',
  'src/lundump.c',
  'src/llex.c',
  'src/lfunc.c',
  'src/lvm.c',
  'src/lapi.c',
  'src/ltable.c',
  dependencies: lua_lib_deps,
  override_options: project_options,
  implicit_include_directories: false,
  include_directories: [inc_inc, inc_src],
)

inc_lib = include_directories('src/lib')
lua_lib = static_library('lualib',
  'src/lib/lstrlib.c',
  'src/lib/lmathlib.c',
  'src/lib/liolib.c',
  'src/lib/ltablib.c',
  'src/lib/lbaselib.c',
  'src/lib/lauxlib.c',
  'src/lib/ldblib.c',
  'src/lib/loadlib.c',
  dependencies: lua_lib_deps,
  override_options: project_options,
  implicit_include_directories: false,
  include_directories: [inc_inc, inc_lib],
)
