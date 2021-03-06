About the Project
#################

:summary: Legal info, third party licenses and contributor credits

`License`_
==========

Magnum library code, documentation and website is licensed under the MIT/Expat
license:

    Copyright © 2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017, 2018
    Vladimír Vondruš <mosra@centrum.cz> and contributors

    Permission is hereby granted, free of charge, to any person obtaining a
    copy of this software and associated documentation files (the "Software"),
    to deal in the Software without restriction, including without limitation
    the rights to use, copy, modify, merge, publish, distribute, sublicense,
    and/or sell copies of the Software, and to permit persons to whom the
    Software is furnished to do so, subject to the following conditions:

    The above copyright notice and this permission notice shall be included
    in all copies or substantial portions of the Software.

    THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
    IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
    FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL
    THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
    LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
    FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER
    DEALINGS IN THE SOFTWARE.

All :dox:`Magnum example code <example-index>` is put into public domain (or
UNLICENSE) to free you from any legal obstacles when reusing the code in your
apps:

    This is free and unencumbered software released into the public domain.

    Anyone is free to copy, modify, publish, use, compile, sell, or distribute
    this software, either in source code form or as a compiled binary, for any
    purpose, commercial or non-commercial, and by any means.

    In jurisdictions that recognize copyright laws, the author or authors of
    this software dedicate any and all copyright interest in the software to
    the public domain. We make this dedication for the benefit of the public
    at large and to the detriment of our heirs and successors. We intend this
    dedication to be an overt act of relinquishment in perpetuity of all
    present and future rights to this software under copyright law.

    THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
    IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
    FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL
    THE AUTHORS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
    IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
    CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

    For more information, please refer to <http://unlicense.org/>

`Third party software`_
=======================

-   Magnum makes use of the **OpenGL** and **WebGL** APIs ---
    https://www.opengl.org/, https://www.khronos.org/webgl/
-   The :dox:`GL` and :dox:`Vk` namespaces internally use code generated using
    the **flextGL** extension loader generator ---
    https://github.com/mosra/flextgl. Copyright © 2011--2018 Thomas Weber,
    licensed under the `MIT license <https://raw.githubusercontent.com/mosra/flextgl/master/COPYING>`_.
-   The :dox:`Audio` namespace depends on the **OpenAL** API ---
    http://www.openal.org.
-   The :dox:`Platform::GlfwApplication` class uses the **GLFW** library ---
    http://www.glfw.org/, licensed under the
    `zlib/libpng <http://www.glfw.org/license.html>`_ license.
-   The :dox:`Platform::GlutApplication` class uses the **freeGLUT** library
    --- http://freeglut.sourceforge.net/, licensed under the MIT license.
-   The :dox:`Platform::Sdl2Application` class uses the **SDL2** library ---
    https://www.libsdl.org/, licensed under the
    `ZLIB license <http://www.gzip.org/zlib/zlib_license.html>`_.

`Contributors`_
===============

Listing only people with code contributions here, otherwise it would be too
many :) Many thanks to everyone involved! Detailed info about contributors to
each particular project can be seen in the ``CREDITS.md`` files in the
:gh:`Corrade <mosra/corrade/blob/master/CREDITS.md>`,
:gh:`Magnum <mosra/magnum/blob/master/CREDITS.md>`,
:gh:`Magnum Plugins <mosra/magnum-plugins/blob/master/CREDITS.md>`,
:gh:`Magnum Integration <mosra/magnum-integration/blob/master/CREDITS.md>`,
:gh:`Magnum Extras <mosra/magnum-extras/blob/master/CREDITS.md>` and
:gh:`Magnum Examples <mosra/magnum-examples/blob/master/CREDITS.md>`
repositories.

.. role:: name
    :class: m-text m-primary
.. role:: gh-name(gh)
    :class: m-flat
.. role:: gh-name-big(gh)
    :class: m-flat m-text m-big

.. todo: reliably disable hyphenation here

.. container:: m-row

    .. container:: m-col-m-8 m-push-m-2 m-nopadt

        .. class:: m-block-dot-t m-spacing-150 m-text-center

        -   :gh-name:`Alexander F Rødseth <xyproto>`
        -   :gh-name:`Alexey Yurchenko <alexesDev>`
        -   :gh-name-big:`Alice Margatroid <alicemargatroid>`
        -   :gh-name:`Andy Somogyi <andysomogyi>`
        -   :gh-name:`ArEnSc`
        -   :gh-name:`Ashwin Ravichandran <ashrko619>`
        -   :gh-name-big:`Bill Robinson <wivlaro>`
        -   :gh-name:`biosek`
        -   :gh-name:`Chris Chambers <chris-chambers>`
        -   :gh-name:`David Lin <davll>`
        -   :gh-name:`Denis Igorevich Lobanov <denislobanov>`
        -   :name:`Gerhard de Clercq`
        -   :gh-name:`dlardi`
        -   :gh-name:`Eliot Saba <staticfloat>`
        -   :gh-name:`Émile Grégoire <emgre>`
        -   :gh-name:`Guillaume Giraud <Guillaume227>`
        -   :gh-name:`Jan Dupal <JanDupal>`
        -   :gh-name:`Janick Martinez Esturo <ph03>`
        -   :gh-name:`jaynus`
        -   :gh-name:`Joel Clay <jclay>`
        -   :gh-name-big:`Jonathan Hale <Squareys>`
        -   :gh-name:`Jonathan Mercier-Ganady <Jmgr>`
        -   :gh-name-big:`Konstantinos Chatzilygeroudis <costashatz>`
        -   :gh-name:`Krzysztof Szenk <Crisspl>`
        -   :gh-name:`Leon Moctezuma <leonidax>`
        -   :gh-name:`Michael Dietschi <mdietsch>`
        -   :name:`Michal Mikula`
        -   :gh-name:`Miguel Martin <miguelmartin75>`
        -   :gh-name:`Nathan Ollerenshaw <matjam>`
        -   :gh-name:`Nicholas "LB" Branden <LB-->`
        -   :gh-name:`Olga Turanksaya <olga-python>`
        -   :gh-name:`Sam Spilsbury <smspillaz>`
        -   :gh-name:`Samuel Kogler <skogler>`
        -   :gh-name:`Séverin Lemaignan <severin-lemaignan>`
        -   :gh-name:`scturtle`
        -   :gh-name:`Siim Kallas <seemk>`
        -   :gh-name:`sigman78`
        -   :gh-name:`Steeve Morin <steeve>`
        -   :gh-name:`Stefan Wasilewski <smw>`
        -   :gh-name:`Tobias Stein <NussknackerXXL>`
        -   :gh-name:`Travis Watkins <amaranth>`
        -   :gh-name:`xantares`

.. class:: m-text m-dim m-small m-text-center

Is this list missing your name or something's wrong?
`Let us know! <{filename}/contact.rst>`_
