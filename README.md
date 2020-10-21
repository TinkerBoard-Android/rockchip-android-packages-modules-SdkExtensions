# SdkExtensions module

SdkExtensions is a module that decides the extension SDK level of the device,
and provides APIs for applications to query the extension SDK level.

## Structure

The module is packaged in an apex, `com.android.sdkext`, and has two components:
- `bin/derive_sdk`: Native binary that runs early in the device boot process and
  reads metadata of other modules, to set system properties relating to the
  extension SDK (for instance `build.version.extensions.r`).
- `javalib/framework-sdkextension.jar`: This is a jar on the bootclasspath that
  exposes APIs to applications to query the extension SDK level.

## Deriving extension SDK level
`derive_sdk` is a program that reads metadata stored in other apex modules, in
the form of binary protobuf files in subpath `etc/sdkinfo.binarypb` inside each
apex. The structure of this protobuf can be seen [here][sdkinfo-proto]. The
exact steps for converting a set of metadata files to actual extension versions
is likely to change over time, and should not be depended upon.

## Reading extension SDK level
The module exposes a java class [`SdkExtensions`][sdkextensions-java] in the
package `android.os.ext`. The method `getExtensionVersion(int)` can be used to
read the version of a particular sdk extension, e.g.
`getExtensionVersion(Build.VERSION_CODES.R)`.

[sdkinfo-proto]: sdk.proto
[sdkextensions-java]: framework/java/android/os/ext/SdkExtensions.java