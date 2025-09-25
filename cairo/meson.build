python_sources = [
  '__init__.py',
  '__init__.pyi',
  'py.typed',
]

sources = [
  'bufferproxy.c',
  'cairomodule.c',
  'context.c',
  'device.c',
  'enums.c',
  'error.c',
  'font.c',
  'glyph.c',
  'matrix.c',
  'misc.c',
  'path.c',
  'pattern.c',
  'rectangle.c',
  'region.c',
  'surface.c',
  'textcluster.c',
  'textextents.c',
]

foreach python_file : python_sources
  fs.copyfile(python_file, python_file)
endforeach

cairo_inc = include_directories('/opt/homebrew/Cellar/cairo/1.18.4/include/cairo')
cairo_lib = cc.find_library('cairo',
    dirs: '/opt/homebrew/Cellar/cairo/1.18.4/lib',
    static: true,
    required: true)

xcb_inc = include_directories('/opt/homebrew/Cellar/libxcb/1.17.0/include/')
xcb_lib = cc.find_library('xcb',
    dirs: '/opt/homebrew/Cellar/libxcb/1.17.0/lib',
    static: true,
    required: true)

x11_inc = include_directories('/opt/homebrew/Cellar/libx11/1.8.12/include')
xorgproto_inc = include_directories('/opt/homebrew/Cellar/xorgproto/2024.1/include')
x11_lib = cc.find_library('X11',
    dirs: '/opt/homebrew/Cellar/libx11/1.8.12/lib',
    static: true,
    required: true)

xrender_lib = cc.find_library('Xrender',
    dirs: '/opt/homebrew/Cellar/libxrender/0.9.12/lib/',
    static: true,
    required: true)

xcb_render_lib = cc.find_library('xcb-render',
    dirs: '/opt/homebrew/Cellar/libxcb/1.17.0/lib/',
    static: true,
    required: true)

xcb_shm_lib = cc.find_library('xcb-shm',
    dirs: '/opt/homebrew/Cellar/libxcb/1.17.0/lib/',
    static: true,
    required: true)

freetype_lib = cc.find_library('freetype',
    dirs: '/opt/homebrew/Cellar/freetype/2.14.1_1/lib/',
    static: true,
    required: true)

fontconfig_lib = cc.find_library('fontconfig',
    dirs: '/opt/homebrew/Cellar/fontconfig/2.17.1/lib/',
    static: true,
    required: true)

xext_lib = cc.find_library('Xext',
    dirs: '/opt/homebrew/Cellar/libxext/1.3.6/lib/',
    static: true,
    required: true)

xau_lib = cc.find_library('Xau',
    dirs: '/opt/homebrew/Cellar/libxau/1.0.12/lib/',
    static: true,
    required: true)

xdmcp_lib = cc.find_library('Xdmcp',
    dirs: '/opt/homebrew/Cellar/libxdmcp/1.1.5/lib/',
    static: true,
    required: true)

gettext_intl_lib = cc.find_library('intl',
    dirs: '/opt/homebrew/Cellar/gettext/0.26/lib/',
    static: true,
    required: true)

pixman_lib = cc.find_library('pixman-1',
    dirs: '/opt/homebrew/Cellar/pixman/0.46.4/lib/',
    static: true,
    required: true)

png_lib = cc.find_library('png16',
    dirs: '/opt/homebrew/Cellar/libpng/1.6.50/lib/',
    static: true,
    required: true)

cairo_dep = declare_dependency(
    include_directories: [cairo_inc, xcb_inc, x11_inc, xorgproto_inc],
    dependencies: [cairo_lib, xcb_lib, x11_lib, xrender_lib, xcb_render_lib, xcb_shm_lib, freetype_lib, fontconfig_lib, xext_lib, xau_lib, xdmcp_lib, gettext_intl_lib, pixman_lib, png_lib])

if cc.get_id() == 'msvc' and not cairo_dep.found()
  if cc.has_header('cairo.h')
    cairo_dep = cc.find_library('cairo')
  endif
endif

python.install_sources(python_sources,
  subdir : 'cairo'
)

install_data(
  'py3cairo.h',
  install_dir: python.get_install_dir(subdir: 'cairo/include')
)
if not for_wheel
  install_headers('py3cairo.h', subdir: 'pycairo')
endif

pyext = python.extension_module('_cairo', sources,
  dependencies : [cairo_dep],
  install: true,
  subdir : 'cairo',
  c_args: pyext_c_args + main_c_args,
)

if not for_wheel
  pkg = import('pkgconfig')

  pkg.generate(
    name: 'py3cairo',
    description: 'Python 3 bindings for cairo',
    subdirs: 'pycairo',
    requires: ['cairo'],
  )
endif

pycairo_dep = declare_dependency(
  version: pycairo_version,
  include_directories: [include_directories('.')],
)
