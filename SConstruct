# SConstruct

import os

env = Environment(CPPPATH=[Dir("include")])

platform = env["PLATFORM"]

# Clang
if platform == "darwin":
    # SCons invokes 'gcc' normally on OS X.
    # Usually this is just clang but with options that we don't need.
    env["CC"] = "clang"
    env.Append(
        CCFLAGS=[
            "-Weverything",
            "-Werror",
            "-O3",
            "-g",
            "-flto",
            "-ffunction-sections",
            "-fdata-sections",
            "-march=native",
        ],
        LINKFLAGS=["-O3", "-g", "-flto", "-dead_strip"],
    )

# GCC
if platform == "posix":
    env.Append(
        CCFLAGS=[
            "-Wall",
            "-Wextra",
            "-Wpedantic",
            "-Wconversion",
            "-Werror",
            "-O3",
            "-g",
            "-flto",
            "-ffunction-sections",
            "-fdata-sections",
            "-fsanitize=address,undefined",
            "-march=native",
        ],
        LINKFLAGS=["-O3", "-g", "-flto", "-Wl,--gc-sections"],
        LIBS=["asan", "ubsan"],
    )

# MSVC
if platform == "win32":
    env.Append(CCFLAGS=["/W4", "/WX", "/Ox"], CPPDEFINES=["_CRT_RAND_S"])

# for color terminal output when available
if "TERM" in os.environ:
    env["ENV"]["TERM"] = os.environ["TERM"]

liblithium = SConscript(
    dirs="src", variant_dir="dist", exports=["env"], duplicate=False
)

test_env = env.Clone()
test_env.Append(LIBS=[liblithium])
SConscript(
    dirs="test", variant_dir="dist/test", exports={"env": test_env}, duplicate=False
)
