Magnum WebGL 1 Info
###################

:css: {filename}/showcase/showcase.css
:highlight: showcase
:breadcrumb: {filename}/showcase.rst Showcase
:alias: /showcase/magnum-gl-info/

Displays various information about Magnum and the WebGL implementation it's
running on. As with the command-line :dox:`magnum-gl-info` utility, it's
possible to specify additional options. Here they are, conveniently linked for
you:

-   `Default view <?>`_
-   `Show limits <?limits>`_
-   `Show all extension strings <?extension-strings>`_

An `asm.js WebGL 1 version <{filename}/showcase/gl-info-asmjs.rst>`_ and
`WebGL 2 version <{filename}/showcase/gl-info-webgl2.rst>`_ is also available.

.. raw:: html

    <div id="container">
      <div id="sizer"><div id="expander"><div id="listener">
        <canvas id="module" class="hidden"></canvas>
        <pre id="log"></pre>
        <div id="status">Initialization...</div>
        <div id="status-description"></div>
        <script async="async" src="{filename}/showcase/gl-info/magnum-gl-info.js"></script>
        <script src="{filename}/showcase/WindowlessEmscriptenApplication.js"></script>
      </div></div></div>
    </div>

.. block-warning:: Doesn't work?

    This example requires `WebAssembly <http://webassembly.org/>`_-capable
    browser with WebGL 1 enabled, but if you don't have a WebAssembly-capable
    browser, you can try to run the classic
    `asm.js version <{filename}/showcase/gl-info-asmjs.rst>`_ instead. See the
    `Showcase <{filename}/showcase.rst>`_ page for more information; you can
    also report a bug either for the :gh:`utility itself <mosra/magnum>` or
    :gh:`for the website <mosra/magnum-website>`. Feedback welcome!

.. block-info:: Documentation

    All options for the Magnum GL Info utility are explained
    :dox:`in the documentation <magnum-gl-info>`.
