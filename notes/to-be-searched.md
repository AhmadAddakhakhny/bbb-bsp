What is IMAGE_FEATURES in yocto?
> it's a well defined feature by meta-oe you can use it instead of using recipes manually, simpily it utilize the foundational features i.e ssh, dbg ...etc.

What is IMAGE_INSTALL in yocto?
> controls what ends up inside the target image’s filesystem, ensuring your custom recipes, libraries, or tools are actually installed on the device.
> IMAGE_INSTALL += " RTUmsg connman hmi "


How to setup qt?
> consider EGLFS as a dependancy for the qt6 (acts as window compesitor)
DISTRO_FEATURES_append = " opengl"
IMAGE_INSTALL_append = " qtbase qtdeclarative qtquickcontrols2"

What is PACKAGECONFIG?
PACKAGECONFIG = “a way to toggle optional features in a recipe, automatically handling their build flags and dependencies.”

PACKAGECONFIG:append:pn-qtbase = " eglfs "