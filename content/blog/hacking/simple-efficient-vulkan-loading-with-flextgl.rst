Simple and efficient Vulkan loading with flextGL
################################################

:date: 2018-05-14
:modified: 2018-06-07
:category: Hacking
:tags: C++, Vulkan, flextGL, OpenGL, Python
:summary: Playing with Vulkan but don't want to include thousands lines of
    various headers just to call a few functions? FlextGL just learned Vulkan
    support and it's here to speed up your turnaround times.

.. role:: wtf
    :class: m-label m-danger
.. role:: lol
    :class: m-label m-warning

.. note-success:: Update: June 07, 2018

    Since the time this article was originally written, I agreed with
    :gh:`ginkgo` to take over flextGL maintainership. The :gh:`mosra/flextGL`
    repository is now the main place. You'll get automatically redirected if
    you have a link to the original location.

If you don't know what :gh:`flextGL <mosra/flextGL>` is, it's a function
loader generator for OpenGL, OpenGL ES and now also Vulkan. In comparison to
GLEW, GL3W, GLAD, glLoadGen and all other function pointer loaders it allows
you to provide a template and a whitelist of versions, extensions and functions
to load, so you can load what you want, however you want.

Chances are you're using flextGL for function pointer loading in your GL / GLES
code, so now you can use the same tool for your Vulkan backend as well.

`How?`_
=======

FlextGL contains a builtin Vulkan template that you can use to generate a basic
loader. In addition you need Python 3 and
`Wheezy Template <https://pypi.org/project/wheezy-template/>`_:

.. code:: sh

    pip3 install wheezy.template
    git clone git://github.com/mosra/flextGL --branch vulkan

Desired Vulkan version, extensions and an optional function white- or blacklist
is specified using a *profile file*:

.. code:: ini

    version 1.0 vulkan

    # Extensions to include on top of the core functionality. The VK_ prefix
    # is omitted.
    extension KHR_swapchain optional
    extension KHR_maintenance1 optional
    # ...

    # Function whitelist. If you omit this section, all functions from the
    # above version and extensions will be pulled in. The vk prefix is omitted.
    begin functions
        CreateInstance
        EnumeratePhysicalDevices
        GetPhysicalDeviceProperties
        GetPhysicalDeviceQueueFamilyProperties
        GetPhysicalDeviceMemoryProperties
        # ...
    end functions

    # You can also choose to have a function blacklist instead, delimited by
    # begin functions blacklist and end functions blacklist.

With a profile file you can then generate the Vulkan loader header + source
file like this. The ``generated/`` directory will then contain a pair of
``flextVk.h`` and ``flextVk.cpp`` files:

.. code:: sh

    ./flextGLgen.py -D generated -t vulkan profile.txt

Now you can just include it and use. The actual function pointer loading is
done by calling :cpp:`flextVkInit()` and after that all Vulkan functions are
available globally:

.. code:: c++

    #include "flextVk.h"

    int main() {
        /* Create an instance, load function pointers */
        VkInstance instance;
        {
            VkInstanceCreateInfo info{};
            info.sType = VK_STRUCTURE_TYPE_INSTANCE_CREATE_INFO;
            vkCreateInstance(&info, nullptr, &instance);
        }
        flextVkInit(instance);

        VkPhysicalDevice physicalDevices[5];
        std::uint32_t count = 5;
        vkEnumeratePhysicalDevices(instance, &count, &physicalDevice);
        ...
    }

`Why bother?`_
==============

Compared to OpenGL, Vulkan is still doing baby steps, however the amount of
available extensions is growing at an alarming rate and soon the size of stock
"all you can eat" headers will have a significant impact on your build times.
Because Vulkan API is more about various types than just function pointers,
flextGL ensures that only the structures, enums and defines that are actually
referenced by functions are pulled in, to shrink header sizes even further.

So, let's have some measurements!

`Header sizes`_
---------------

The following table compares raw line count and line count of preprocessed
output when using various Vulkan loaders, generated by the following two
commands using GCC 7.3.1 for Vulkan 1.1.74:

.. code:: sh

    wc -l /path/to/header

    echo "#include <header>" | g++ -std=c++11 -E -x c++ - | wc -l

.. class:: m-table

======================================================= ========= ===========
Header                                                  Line      After
                                                        count     preprocessing
======================================================= ========= ===========
:cpp:`#include "flextVk.h"` [1]_                        1 710     1 929
:cpp:`#include <MagnumExternal/Vulkan/flextVk.h>` [2]_  3 577     3 592
:cpp:`#include "volk.h"`                                837 [3]_  6 352
:cpp:`#include <vulkan/vulkan.h>` [4]_                  7 470     7 363
:cpp:`#include <vulkan/vulkan.hpp>` [5]_                42 544    83 530
                                                        :wtf:`!`  :wtf:`!!`
:cpp:`#include <GL/glew.h>` (for comparison)            23 686    7 464
                                                        :lol:`?!`
======================================================= ========= ===========

.. [1] A minimal generated header whitelisting only functions required to build
    my `First Triangle in Vulkan <https://twitter.com/czmosra/status/970601850348212225>`_. The profile file used to generate the header is
    included `in the gist <https://gist.github.com/mosra/268defb3ffbb0f2cd78815394f27f8a3#file-profile-flextgl-txt>`_.
.. [2] Experimental Vulkan header included in latest Magnum master, including
    everything from Vulkan 1.1 + all extensions that were promoted to 1.1 for
    backwards compatibility
.. [3] The `Volk <https://github.com/zeux/volk>`_ meta-loader. While small on
    its own, it depends on the stock ``vulkan.h`` for all type and enum
    definitions
.. [4] The stock Vulkan header provides only function pointer typedefs, not
    actual functions, so can't be used as-is. The ``vulkan.h`` header itself
    has only 79 lines, this counts lines of ``vulkan_core.h``.
.. [5] `vulkan.hpp <https://github.com/KhronosGroup/Vulkan-Hpp>`_, aiming to
    provide C++11 header-only Vulkan "bindings" with better type safety. But,
    *look at those numbers*, seriously, don't use this thing. *Please.*

`Compile times`_
----------------

I abused :dox:`Corrade::TestSuite <TestSuite-Tester-benchmark>` and
:cpp:`std::system()` a bit to benchmark how long it takes GCC to compile each
case from the above table into an executable that creates the Vulkan instance
and populates function pointers using given loader. Only compilation of the
actual main file is measured, excluding time needed to compile extra ``*.cpp``,
``*.c``  or ``*.so`` files, because their cost is usually amortized in the
project. Here are the results (hover over the bars to get the concrete values):

.. plot:: Compile time
    :type: barh
    :labels:
        flextVk minimal
        flextVk Magnum
        Volk
        vulkan.h
        vulkan.hpp
    :labels_extra:
        1929 lines
        3592 lines
        6352 lines
        7363 lines
        83530 lines
    :units: ms
    :values: 62.69 69.98 74.78 76.76 719.71
    :errors: 0.84 2.04 3.65 3.34 6.95
    :colors: success success info info danger

As expected, ``vulkan.hpp`` takes an *insane* amount of time to compile ---
**ten times as much** as the others, almost a second --- and this is for every
file that (transitively) includes it! The compile time roughly corresponds to
preprocessed line count from the above table, with flextGL-generated headers
being the smallest and fastest to compile.

As is usual, the headers usually get transitively included into majority of
a project, so saving 15 milliseconds per file when going from stock headers
to flextGL-generated ones can save you 15 seconds in moderately sized project
having 1000 targets. And this gap will be increasing as more extensions get
added to the stock headers.

`Runtime cost`_
---------------

Because flextGL loads only the functions you actually requested instead of
everything that anybody could ever need, it has also some impact on startup
time. The following benchmark measures the time it takes to call
loader-specific initialization functions. The ``vulkan.h`` and ``vulkan.hpp``
headers aren't included, because these rely on external function pointer
loading and don't do any on their own.

.. plot:: Runtime cost
    :type: barh
    :labels:
        flextVk minimal
        flextVk Magnum
        Volk
        vkCreateInstance()
    :labels_extra:
        49 ptrs
        192 ptrs
        302 ptrs
        (for comparison)
    :units: µs
    :values: 15.09 84.98 197.13 934.27
    :errors: 0.74 3.65 9.45 25.66
    :colors: success success info dim

Again, the measured time corresponds to actual amount of loaded function
pointers. The Vulkan Triangle needs just 49 function pointers,
Magnum loads everything from Vulkan 1.1 together with command aliases from
promoted extensions, while Volk adds also all known extensions. However, note
that these are *microseconds* --- and compared to time that's needed to create
a Vulkan instance (last measurement), the savings are only very minor.

`Vulkan loading in Magnum`_
===========================

As of :gh:`mosra/magnum@b1377033e81efd5f3037b8624cf4574bd3574d52`, Magnum
ships flextGL-generated Vulkan headers. To save on delegation overhead, the
decision was to load per-device function pointers instead of going through
per-instance function pointers for everything --- that's also what `Volk`_
does with great success, saving as much as 5% to 10% of driver overhead,
depending on the workflow.

Besides that, loaded Vulkan functions are not global by default in order to
support multiple coexisting Vulkan instances:

.. code:: c++

    #include <MagnumExternal/Vulkan/flextVk.h>

    int main() {
        /* Create an instance */
        VkInstance instance;
        {
            VkInstanceCreateInfo info{};
            info.sType = VK_STRUCTURE_TYPE_INSTANCE_CREATE_INFO;
            // ...
            vkCreateInstance(&info, nullptr, &instance);
        }

        /* Load per-instance function pointers */
        FlextVkInstance i;
        flextVkInitInstance(instance, &i);

        /* Create a device */
        VkPhysicalDevice physicalDevice;
        {
            uint32_t count = 1;
            i.EnumeratePhysicalDevices(instance, &count, &physicalDevice);
        }
        VkDevice device;
        {
            VkDeviceCreateInfo info{};
            info.sType = VK_STRUCTURE_TYPE_DEVICE_CREATE_INFO;
            // ...
            i.CreateDevice(physicalDevice, &info, nullptr, &device);
        }

        /* Load per-device function pointers */
        FlextVkDevice d;
        flextVkInitDevice(device, &d, i.GetDeviceProcAddr);

        // ...
    }

In the above snippet, the ``i`` and ``d`` structures contain all loaded
function pointers. So instead of :cpp:`vkCreateBuffer(device, ...)` you'd write
:cpp:`d.Createbuffer(device, )`, for example. While this is properly decoupled,
it might get in the way when just playing around or adapting sample code. For
that reason, Magnum provides opt-in global function pointers as well --- just
include ``flextVkGlobal.h`` instead of ``flextVk.h`` and load your pointers
globally:

.. code:: c++

    #include <MagnumExternal/Vulkan/flextVkGlobal.h>

    int main() {
        /* Create an instance */
        VkInstance instance;
        {
            VkInstanceCreateInfo info{};
            info.sType = VK_STRUCTURE_TYPE_INSTANCE_CREATE_INFO;
            // ...
            vkCreateInstance(&info, nullptr, &instance);
        }

        /* Load per-instance function pointers globally */
        flextVkInitInstance(instance, &flextVkInstance);

        /* Create a device */
        VkPhysicalDevice physicalDevice;
        {
            uint32_t count = 1;
            vkEnumeratePhysicalDevices(instance, &count, &physicalDevice);
        }
        VkDevice device;
        {
            VkDeviceCreateInfo info{};
            info.sType = VK_STRUCTURE_TYPE_DEVICE_CREATE_INFO;
            // ...
            vkCreateDevice(physicalDevice, &info, nullptr, &device);
        }

        /* Load per-device function pointers globally */
        flextVkInitDevice(device, &flextVkDevice, vkGetDeviceProcAddr);

        // ...
    }

In this case :cpp:`flextVkInitInstance()` and :cpp:`flextVkInitDevice()` will
load the pointers into global :cpp:`flextVkInstance` and :cpp:`flextVkDevice`
structures, which then are aliases to global ``vk*()`` functions.

Both approaches can coexist, just be sure that you call
instance-/device-specific functions on the instance/device that they were
queried from and everything will work well.

.. transition:: ~ ~ ~

And that's it! Check Vulkan support in flextGL out and please report bugs, if
you find any. Thanks for reading, I'll be back soon!

.. note-dim::

    Discussion: `Twitter <https://twitter.com/czmosra/status/996203184644386821>`_,
    Reddit `r/vulkan <https://www.reddit.com/r/vulkan/comments/8jhrii/simple_and_efficient_vulkan_loading_with_flextgl/>`_ and
    `r/gamedev <https://www.reddit.com/r/gamedev/comments/8jhsc5/simple_and_efficient_vulkan_api_loading_with/>`_
