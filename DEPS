# Note: The buildbots evaluate this file with CWD set to the parent
# directory and assume that the root of the checkout is in ./v8/, so
# all paths in here must match this assumption.

vars = {
  "git_url": "https://chromium.googlesource.com",
}

deps = {
  "v8/build/gyp":
    Var("git_url") + "/external/gyp.git" + "@" + "4d7c139b1820c5fcb993868c61f170a02cda8a40",
  "v8/third_party/icu":
    Var("git_url") + "/chromium/deps/icu.git" + "@" + "4e3266f32c62d30a3f9e2232a753c60129d1e670",
  "v8/buildtools":
    Var("git_url") + "/chromium/buildtools.git" + "@" + "da0df3fdac6036e862addb1155a2d6c11b6c18d5",
  "v8/testing/gtest":
    Var("git_url") + "/external/googletest.git" + "@" + "be1868139ffe0ccd0e8e3b37292b84c821d9c8ad",
  "v8/testing/gmock":
    Var("git_url") + "/external/googlemock.git" + "@" + "29763965ab52f24565299976b936d1265cb6a271",  # from svn revision 501
  "v8/tools/clang":
    Var("git_url") + "/chromium/src/tools/clang.git" + "@" + "5489bbf01a5292a6c44d4cad7875ba2cb4ac975b",
}

deps_os = {
  "android": {
    "v8/third_party/android_tools":
      Var("git_url") + "/android_tools.git" + "@" + "4f723e2a5fa5b7b8a198072ac19b92344be2b271",
  },
  "win": {
    "v8/third_party/cygwin":
      Var("git_url") + "/chromium/deps/cygwin.git" + "@" + "c89e446b273697fadf3a10ff1007a97c0b7de6df",
  }
}

include_rules = [
  # Everybody can use some things.
  "+include",
  "+unicode",
  "+third_party/fdlibm",
]

# checkdeps.py shouldn't check for includes in these directories:
skip_child_includes = [
  "build",
  "third_party",
]

hooks = [
  # Pull clang-format binaries using checked-in hashes.
  {
    "name": "clang_format_win",
    "pattern": ".",
    "action": [ "download_from_google_storage",
                "--no_resume",
                "--platform=win32",
                "--no_auth",
                "--bucket", "chromium-clang-format",
                "-s", "v8/buildtools/win/clang-format.exe.sha1",
    ],
  },
  {
    "name": "clang_format_mac",
    "pattern": ".",
    "action": [ "download_from_google_storage",
                "--no_resume",
                "--platform=darwin",
                "--no_auth",
                "--bucket", "chromium-clang-format",
                "-s", "v8/buildtools/mac/clang-format.sha1",
    ],
  },
  {
    "name": "clang_format_linux",
    "pattern": ".",
    "action": [ "download_from_google_storage",
                "--no_resume",
                "--platform=linux*",
                "--no_auth",
                "--bucket", "chromium-clang-format",
                "-s", "v8/buildtools/linux64/clang-format.sha1",
    ],
  },
  # Pull binutils for linux, enabled debug fission for faster linking /
  # debugging when used with clang on Ubuntu Precise.
  # https://code.google.com/p/chromium/issues/detail?id=352046
  {
    'name': 'binutils',
    'pattern': 'v8/third_party/binutils',
    'action': [
        'python',
        'v8/third_party/binutils/download.py',
    ],
  },
  {
    # Pull clang if needed or requested via GYP_DEFINES.
    # Note: On Win, this should run after win_toolchain, as it may use it.
    'name': 'clang',
    'pattern': '.',
    'action': ['python', 'v8/tools/clang/scripts/update.py', '--if-needed'],
  },
  {
    # A change to a .gyp, .gypi, or to GYP itself should run the generator.
    "pattern": ".",
    "action": ["python", "v8/build/gyp_v8"],
  },
]
