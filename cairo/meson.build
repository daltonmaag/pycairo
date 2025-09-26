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

cairo_dep = dependency('cairo',
  version: cair_version_req,
  required: cc.get_id() == 'msvc',
  fallback: ['cairo', 'libcairo_dep'],
  default_options: [
    'freetype=enabled',
    'glib=disabled',
    'gtk_doc=false',
    'gtk2-utils=disabled',
    'spectre=disabled',
    'tests=disabled',
    'xcb=disabled',
    'xlib-xcb=disabled',
    'xlib=disabled',
  ],
)

# When hardcoding the use of the subproject:
# cairo_subproject = subproject(
#   'cairo',
#   default_options: [
#     'freetype=enabled',
#     'glib=disabled',
#     'gtk_doc=false',
#     'gtk2-utils=disabled',
#     'spectre=disabled',
#     'tests=disabled',
#     'xcb=disabled',
#     'xlib-xcb=disabled',
#     'xlib=disabled',
#   ]
# )
# cairo_dep = cairo_subproject.get_variable('libcairo_dep')

# XXX: Not sure what to do with this.
# if cc.get_id() == 'msvc' and not cairo_dep.found()
#   if cc.has_header('cairo.h')
#     cairo_dep = cc.find_library('cairo')
#   endif
# endif

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

pyext = python.extension_module(
  '_cairo',
  sources,
  dependencies: [cairo_dep],
  install: true,
  subdir: 'cairo',
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
