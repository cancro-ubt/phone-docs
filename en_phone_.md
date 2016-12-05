





# Ubuntu phone development

## Get started

Here you can install everything needed to get developing apps and scopes for
Ubuntu.

  1. Start by [Installing the Ubuntu SDK](/phone/platform/sdk/installing-the-sdk/).
  2. Check out the [Ubuntu installation guide for devices](/phone/devices/installing-ubuntu-for-devices/) to install Ubuntu on a supported device.

**Tip**: A device is not required: you can develop and run apps and scopes using the Ubuntu emulator right in your Ubuntu desktop. For more info, see [Ubuntu SDK](/phone/platform/sdk/)

![](/static/devportal_uploaded/465794a5-c1fe-4582-80c9-c0fa9363f552-cms_page_media/1141/tel3.png)





## Pick your Ubuntu Phone adventure

### Apps

Mobile and convergent apps written using QML, HTML5 and Cordova, with access
to a wide range of device and system APIs.

[Learn more about Apps ›](/phone/apps)

### Scopes

Dedicated views on the homescreen to find, organize and show a variety of
content types, originating on the Internet or the local device.

[Learn more about Scopes ›](/phone/scopes)





## The Ubuntu development model

### Frameworks: targetting APIs

Ubuntu applications and scopes are packaged, distributed and deployed using a
format called `click` packaging. A framework is a set of Ubuntu APIs an app or
scope is developed for.

When packaged, all apps and scopes must declare which API framework they are
intending to use on the device.

[Learn more about frameworks ›](/phone/platform/guides/frameworks/)

### Security and app isolation

All Ubuntu apps and scopes are confined, meaning they only have access to
their own resources and are isolated from other apps and parts of the system.
The developer must declare which policy groups are needed for the app or scope
to function properly within the confinement rules providing security and
privacy.

[Learn more about security policies ›](/phone/platform/guides/app-confinement/)

### The build environment

A build environment, or click target, will be required to develop and test an
app or scope. This environment will make it possible to build the software for
a different architecture if cross-compilation is required (e.g. an app that
uses C++) and to run it on different devices (the desktop, a phone/tablet or
the emulator). Whenever a target is required the IDE will help to configure it
based on the framework and target architecture (e.g. i386 or armhf). The
architecture will correspond to the test environment the developer is using
and ultimately what the products are built with.

[Learn more about building for different architectures›](/phone/apps/sdk/tutorials/building-cross-architecture-click-applications/)

### Testing applications on devices

As far as testing environments, the developer can choose an **Ubuntu
emulator**, which can be x86 or armhf, or real hardware with a **reference
device**, such as the Nexus 4 or Nexus 7. While it is possible that simple
apps may work in the local desktop environment, it is only in one of these
supported testing environments that the entire set of framework APIs are
available. It is generally recommended that an app or scope be packaged as a
click and installed to the device or emulator in order to properly test it.
Again the IDE will assist with creating, validating, deploying and installing
the package.

[Learn how to run apps with the Ubuntu SDK IDE›](/phone/apps/sdk/tutorials/running-apps-from-the-sdk/)





