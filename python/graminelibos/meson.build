conf = configuration_data()
conf.set('VERSION', meson.project_version())
conf.set('PKGLIBDIR', get_option('prefix') / pkglibdir)
conf.set('LIBDIR', get_option('prefix') / get_option('libdir'))
conf.set('SYSLIBDIR', syslibdir)
conf.set('SGX_ENABLED', sgx ? '1' : '0')

init_py = configure_file(
    input: '__init__.py',
    output: '__init__.py',
    configuration: conf,
)

python_src = [
    init_py,
    'gen_jinja_env.py',
    'manifest.py',
    'manifest_check.py',
]

if enable_tests
    python_src += [
        'ninja_syntax.py',
        'regression.py',
        'util_tests.py',
    ]
endif

if sgx
    python_src += [
        'sgx_sign.py',
        'sigstruct.py',
    ]
endif

install_data(python_src, install_dir: python3_pkgdir)
