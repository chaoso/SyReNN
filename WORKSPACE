workspace(name = "custom_relu_transformer")

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

# Get the Python interpreter & build dependency (OpenSSL).
# https://stackoverflow.com/questions/47036855
http_archive(
    name = "openssl",
    urls = ["https://www.openssl.org/source/openssl-1.0.2s.tar.gz"],
    strip_prefix = "openssl-1.0.2s",
    build_file = "openssl.BUILD",
    sha256 = "cabd5c9492825ce5bd23f3c3aeed6a97f8142f606d893df216411f07d1abab96",
)
http_archive(
    name = "python_3",
    urls = ["https://www.python.org/ftp/python/3.7.4/Python-3.7.4.tgz"],
    strip_prefix = "Python-3.7.4",
    build_file = "python.BUILD",
    sha256 = "d63e63e14e6d29e17490abbe6f7d17afb3db182dbd801229f14e55f4157c4ba3",
)
register_toolchains("//:python3-toolchain")

load("@bazel_tools//tools/build_defs/repo:git.bzl", "git_repository")

git_repository(
    name = "io_bazel_rules_python",
    remote = "https://github.com/bazelbuild/rules_python.git",
    commit = "d852e8a063b83f1c75c2cf0bcba1a0f9e54e62d0",
    shallow_since = "1564152315 -0400",
)

# Only needed for PIP support:
load("@io_bazel_rules_python//python:pip.bzl", "pip_repositories")
pip_repositories()
load("@io_bazel_rules_python//python:pip.bzl", "pip_import")

# EIGEN SUPPORT
# See the README in: https://github.com/bazelbuild/rules_foreign_cc
# Group the sources of the library so that CMake rule have access to it
all_content = """filegroup(name = "all", srcs = glob(["**"]), visibility = ["//visibility:public"])"""

# Rule repository
http_archive(
   name = "rules_foreign_cc",
   strip_prefix = "rules_foreign_cc-adb04eed2c058b4b161b321b1104eb55ef63b0f4",
   url = "https://github.com/bazelbuild/rules_foreign_cc/archive/adb04eed2c058b4b161b321b1104eb55ef63b0f4.zip",
   sha256 = "82811053dbeb6cefabc9e69c6775ae45d4a13ebfc7c301cc3f6df3eb40947e99"
)

load("@rules_foreign_cc//:workspace_definitions.bzl", "rules_foreign_cc_dependencies")

rules_foreign_cc_dependencies()

# OpenBLAS, Eigen, and MKLDNN source code repositories
http_archive(
   name = "openblas",
   build_file = "openblas.BUILD",
   strip_prefix = "OpenBLAS-744779d3351aa96afa5188d75eb76ac5ec197d13",
   urls = ["https://github.com/xianyi/OpenBLAS/archive/744779d3351aa96afa5188d75eb76ac5ec197d13.tar.gz"],
   sha256 = "488294c6176bd0318b2453d0cff203a31f76a336a06c48f8ae7a23e46bf4d5df",
)
http_archive(
   name = "eigen",
   build_file = "eigen.BUILD",
   strip_prefix = "eigen-git-mirror-3.3.5",
   urls = ["https://github.com/eigenteam/eigen-git-mirror/archive/3.3.5.tar.gz"],
   sha256 = "992855522dfdd0dea74d903dcd082cdb01c1ae72be5145e2fe646a0892989e43",
)
http_archive(
   name = "mkldnn",
   build_file = "mkldnn.BUILD",
   strip_prefix = "mkl-dnn-1.0.1",
   urls = ["https://github.com/intel/mkl-dnn/archive/v1.0.1.tar.gz"],
   sha256 = "91fb84601c18f8a5a87eccd7b63d61f03495f36c5c533bd7f59443e4f8bb2595",
)
http_archive(
   name = "tbb",
   build_file = "tbb.BUILD",
   strip_prefix = "tbb-2019_U8",
   urls = ["https://github.com/intel/tbb/archive/2019_U8.tar.gz"],
   sha256 = "7b1fd8caea14be72ae4175896510bf99c809cd7031306a1917565e6de7382fba",
)

# GOOGLETEST
# https://docs.bazel.build/versions/master/cpp-use-cases.html#writing-and-running-c-tests
http_archive(
    name = "gtest",
    url = "https://github.com/google/googletest/archive/release-1.7.0.zip",
    sha256 = "b58cb7547a28b2c718d1e38aee18a3659c9e3ff52440297e965f5edffe34b6d0",
    build_file = "gtest.BUILD",
    strip_prefix = "googletest-release-1.7.0",
)

# PROTOBUF SUPPORT
# See https://github.com/bazelbuild/bazel/issues/6984#issuecomment-449472008
http_archive(
    name = "com_google_protobuf",
    sha256 = "2ee9dcec820352671eb83e081295ba43f7a4157181dad549024d7070d079cf65",
    strip_prefix = "protobuf-3.9.0",
    urls = ["https://github.com/protocolbuffers/protobuf/archive/v3.9.0.tar.gz"],
)
load("@com_google_protobuf//:protobuf_deps.bzl", "protobuf_deps")
protobuf_deps()

# PROTOBUF HELPER RULES, supports GRPC services
http_archive(
    name = "build_stack_rules_proto",
    urls = ["https://github.com/stackb/rules_proto/archive/d9a123032f8436dbc34069cfc3207f2810a494ee.tar.gz"],
    sha256 = "85ccc69a964a9fe3859b1190a7c8246af2a4ead037ee82247378464276d4262a",
    strip_prefix = "rules_proto-d9a123032f8436dbc34069cfc3207f2810a494ee",
)
load("@build_stack_rules_proto//cpp:deps.bzl", "cpp_grpc_library")
cpp_grpc_library()
load("@build_stack_rules_proto//python:deps.bzl", "python_grpc_library")
python_grpc_library()

pip_import(
    name = "protobuf_py_deps",
    requirements = "@build_stack_rules_proto//python/requirements:protobuf.txt",
)
load("@protobuf_py_deps//:requirements.bzl", protobuf_pip_install = "pip_install")
protobuf_pip_install()

pip_import(
    name = "grpc_py_deps",
    requirements = "@build_stack_rules_proto//python:requirements.txt",
)
load("@grpc_py_deps//:requirements.bzl", grpc_pip_install = "pip_install")
grpc_pip_install()

# GRPC SUPPORT
http_archive(
    name = "com_github_grpc_grpc",
    urls = ["https://github.com/grpc/grpc/archive/119be19126ef16ac41859e5e1e88a69fcd644bed.tar.gz"],
    strip_prefix = "grpc-119be19126ef16ac41859e5e1e88a69fcd644bed",
)
load("@com_github_grpc_grpc//bazel:grpc_deps.bzl", "grpc_deps")
grpc_deps()

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_file")

# Plane image from Wikimedia Commons.
http_file(
    name = "plane_svg",
    urls = ["https://upload.wikimedia.org/wikipedia/commons/9/95/Plane_icon_nose_down.svg"],
    downloaded_file_path = "plane.svg",
    sha256 = "f4bec6365e51afdba8fa254247d44e19f3beb81eadfe369ec0867ee9959d1a49",
)

# From https://github.com/eth-sri/eran

# TODO(masotoud): Find a way to not have to do all of these explicitly. Perhaps
# download a zip of the ERAN repo and just pull out the models? This will
# require modifying the run_analysis, etc. code re: where it pulls models from.

# MODELS
http_file(
    name = "mnist_relu_3_100_model",
    urls = ["https://files.sri.inf.ethz.ch/eran/nets/tensorflow/mnist/mnist_relu_3_100.tf"],
    downloaded_file_path = "model.eran",
    sha256 = "e4151dfced1783360ab8353c8fdedbfd76f712c2c56e4b14799b2f989217229f",
)

http_file(
    name = "mnist_relu_4_1024_model",
    urls = ["https://files.sri.inf.ethz.ch/eran/nets/tensorflow/mnist/mnist_relu_4_1024.tf"],
    downloaded_file_path = "model.eran",
    sha256 = "03b53b317dfd0a05a72a7566d80d7fc4756a2e664c93c93459b12cd7b886d810",
)

http_file(
    name = "mnist_relu_convsmall_model",
    urls = ["https://files.sri.inf.ethz.ch/eran/nets/pytorch/mnist/convSmallRELU__Point.pyt"],
    downloaded_file_path = "model.eran",
    sha256 = "87953b7f8412091e9eebaae7c56e5432b4a1559b6290676c559f4a7f9825cfc7",
)

http_file(
    name = "mnist_relu_convsmall_diffai_model",
    urls = ["https://files.sri.inf.ethz.ch/eran/nets/pytorch/mnist/convSmallRELU__DiffAI.pyt"],
    downloaded_file_path = "model.eran",
    sha256 = "76371053d6faac369858769a25b845bfd177bd88a693b3c49e4590ee3f2fe721",
)

http_file(
    name = "mnist_relu_convsmall_pgd_model",
    urls = ["https://files.sri.inf.ethz.ch/eran/nets/pytorch/mnist/convSmallRELU__PGDK.pyt"],
    downloaded_file_path = "model.eran",
    sha256 = "1cad08171eb1651e49aa8ae13855c3c3dfa125dac32f7187253139d267e43f7e",
)

http_file(
    name = "mnist_relu_convmedium_model",
    urls = ["https://files.sri.inf.ethz.ch/eran/nets/pytorch/mnist/convMedGRELU__Point.pyt"],
    downloaded_file_path = "model.eran",
    sha256 = "88c414a0f69a3469731c45bc14d9583cb8189fbbbab003ca70d818a041a31103",
)

http_file(
    name = "mnist_relu_convmedium_diffai_model",
    urls = ["https://files.sri.inf.ethz.ch/eran/nets/pytorch/mnist/convMedGRELU__DiffAI.pyt"],
    downloaded_file_path = "model.eran",
    sha256 = "",
)


http_file(
    name = "cifar10_relu_4_100_model",
    urls = ["https://files.sri.inf.ethz.ch/eran/nets/tensorflow/cifar/cifar_relu_4_100.tf"],
    downloaded_file_path = "model.eran",
    sha256 = "9f72f9f798109264c02d4d8ad8d7d2e6fcfd8d474fb7e0f9b482741279a4d780",
)

http_file(
    name = "cifar10_relu_9_200_model",
    urls = ["https://files.sri.inf.ethz.ch/eran/nets/tensorflow/cifar/cifar_relu_9_200.tf"],
    downloaded_file_path = "model.eran",
    sha256 = "46964f58e6fce152a51e0fe69620c91527497eb0152ea95d926f167b88c67d01",
)

http_file(
    name = "cifar10_relu_convsmall_model",
    urls = ["https://files.sri.inf.ethz.ch/eran/nets/pytorch/cifar/convSmallRELU__Point.pyt"],
    downloaded_file_path = "model.eran",
    sha256 = "e486e5adc14ec474beeddeabf71ceb6aa50aa4e789ab23d4ebb9df71e58eadaa",
)

http_file(
    name = "cifar10_relu_convsmall_diffai_model",
    urls = ["https://files.sri.inf.ethz.ch/eran/nets/pytorch/cifar/convSmallRELU__DiffAI.pyt"],
    downloaded_file_path = "model.eran",
    sha256 = "e9dceef1ed18b488ffbfa22649725d7fdb176fa97e06fd47101015f94d62313d",
)

http_file(
    name = "cifar10_relu_convsmall_pgd_model",
    urls = ["https://files.sri.inf.ethz.ch/eran/nets/pytorch/cifar/convSmallRELU__PGDK.pyt"],
    downloaded_file_path = "model.eran",
    sha256 = "83e7b7e426a35da490924947eb7792634bced5f9723cf24d0c238624122b6fa8",
)

http_file(
    name = "cifar10_relu_convmedium_model",
    urls = ["https://files.sri.inf.ethz.ch/eran/nets/pytorch/cifar/convMedGRELU__Point.pyt"],
    downloaded_file_path = "model.eran",
    sha256 = "136df5d070cef55268316965a2aa1bc68c0b61da4685a331619ee3d8a9e97e50",
)

http_file(
    name = "cifar10_relu_convbig_diffai_model",
    urls = ["https://files.sri.inf.ethz.ch/eran/nets/pytorch/cifar/convBigRELU__DiffAI.pyt"],
    downloaded_file_path = "model.eran",
    sha256 = "f2265c24149ba0ba04480a315d54e2301870149f90da039418b31332d03781a0",
)

# ACAS MODELS
http_archive(
   name = "reluplex",
   urls = ["https://github.com/guykatzz/ReluplexCav2017/archive/c6fcc3308c3edcf1c155e2b9625ac924d06e099b.zip"],
   strip_prefix = "ReluplexCav2017-c6fcc3308c3edcf1c155e2b9625ac924d06e099b",
   build_file_content = all_content,
   sha256 = "d058b1a7873416f2581a975e0099f5145888faf711b6f7e9b8f1a9b61d75f203",
)

# ONNX MODELS
http_archive(
   name = "onnx_squeezenet",
   strip_prefix = "squeezenet1.1",
   urls = ["https://s3.amazonaws.com/onnx-model-zoo/squeezenet/squeezenet1.1/squeezenet1.1.tar.gz"],
   build_file_content = all_content,
   sha256 = "aff6280d73c0b826f088f7289e4495f01f6e84ce75507279e1b2a01590427723",
)

http_archive(
   name = "onnx_vgg16",
   strip_prefix = "vgg16",
   urls = ["https://s3.amazonaws.com/onnx-model-zoo/vgg/vgg16/vgg16.tar.gz"],
   build_file_content = all_content,
   sha256 = "67ae525d46b971bde45c7e5830c50f09ce4c7cdc97bf6dce48a4238a9b69efba",
)

# DATASETS
http_file(
    name = "mnist_test_data",
    urls = ["https://raw.githubusercontent.com/eth-sri/eran/16efb21c401c6912327d6fb70b237ae7a8a1ccd9/data/mnist_test.csv"],
    downloaded_file_path = "data.csv",
    sha256 = "91059c3f780826c6474841e3ec7ca6aad4c89878753a885704c868306573dd29",
)

http_file(
    name = "cifar10_test_data",
    urls = ["https://raw.githubusercontent.com/eth-sri/eran/16efb21c401c6912327d6fb70b237ae7a8a1ccd9/data/cifar10_test.csv"],
    downloaded_file_path = "data.csv",
    sha256 = "4cef6cdf54a6b78c7cae5ac7d20dc1d6eda1c9d8a04c6b08b5ecba6029a6f043",
)

http_archive(
    name = "imagenette",
    strip_prefix = "imagenette-320",
    urls = ["https://s3.amazonaws.com/fast-ai-imageclas/imagenette-320.tgz"],
    build_file_content = all_content,
    sha256 = "0e3ac85d7f3fd0a63d5e27d85c68511be9ac4dae260345b6f7434a0d4fa24b3f",
)