workspace(
    name = "quarkus-bazel-gradle"
)


load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

RULES_GRADLE_EXTERNAL_TAG = "0.3"
RULES_GRADLE_EXTERNAL_SHA = "1bfa34271793980c9709fc49561450f97f659c4bf3c8404028b740dff1a2a669"

http_archive(
    name = "rules_gradle",
    strip_prefix = "rules_gradle-%s" % RULES_GRADLE_EXTERNAL_TAG,
    sha256 = RULES_GRADLE_EXTERNAL_SHA,
    url = "https://github.com/carolosf/rules_gradle/archive/refs/tags/%s.zip" % RULES_GRADLE_EXTERNAL_TAG
)

#local_repository(
#    name = "rules_gradle",
#    path = "/home/username/dev/rules_gradle",
#)

GRADLE_VERSION = "7.4.2"
GRADLE_SHA = "29e49b10984e585d8118b7d0bc452f944e386458df27371b49b4ac1dec4b7fda"

http_archive(
    name = "gradle",
    build_file = "@rules_gradle//:BUILD",
    strip_prefix = "gradle-%s" % GRADLE_VERSION,
    sha256 = GRADLE_SHA,
    url = "https://services.gradle.org/distributions/gradle-%s-bin.zip" % GRADLE_VERSION,
)

# Start of optional part -----------------------------------------------------------------------------------------------
# If you want to build hermetic docker images then you will need the following:

# For rules_docker from https://github.com/bazelbuild/rules_docker
# io_bazel_rules_docker has an issue with m1 macs - x86 version of bazel works ok :(
http_archive(
    name = "io_bazel_rules_docker",
    sha256 = "b1e80761a8a8243d03ebca8845e9cc1ba6c82ce7c5179ce2b295cd36f7e394bf",
    urls = ["https://github.com/bazelbuild/rules_docker/releases/download/v0.25.0/rules_docker-v0.25.0.tar.gz"],
)
load("@io_bazel_rules_docker//toolchains/docker:toolchain.bzl",
    docker_toolchain_configure="toolchain_configure"
)

load(
    "@io_bazel_rules_docker//repositories:repositories.bzl",
    container_repositories = "repositories",
)
container_repositories()

load("@io_bazel_rules_docker//repositories:deps.bzl", container_deps = "deps")

container_deps()

load(
    "@io_bazel_rules_docker//container:container.bzl",
    "container_pull",
)

container_pull(
    name = "openjdk-11",
    registry = "registry.access.redhat.com",
    repository = "ubi8/openjdk-11",
    tag = "1.11"
)
# End of optional part -------------------------------------------------------------------------------------------------