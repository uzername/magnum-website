Object Picking Example
######################

:css: {filename}/showcase/showcase.css
:highlight: showcase
:breadcrumb: {filename}/showcase.rst Showcase

Showcases usage of multiple framebuffer attachments to implement object
picking.

.. topic:: Key controls

    -   :label-default:`Mouse drag` --- rotate the scene around
    -   :label-default:`Mouse click` --- select an object

.. raw:: html

    <div id="container">
      <div id="sizer"><div id="expander"><div id="listener">
        <canvas id="module" tabindex="0"></canvas>
        <div id="status">Initialization...</div>
        <div id="status-description"></div>
        <script async="async" src="{filename}/showcase/picking/magnum-picking.js"></script>
        <script src="{filename}/showcase/EmscriptenApplication.js"></script>
      </div></div></div>
    </div>

.. block-warning:: Doesn't work?

    This example requires `WebAssembly <http://webassembly.org/>`_-capable
    browser with WebGL 1 enabled. See the `Showcase <{filename}/showcase.rst>`_
    page for more information; you can also report a bug either for the
    :gh:`example itself <mosra/magnum-examples>` or
    :gh:`for the website <mosra/magnum-website>`. Feedback welcome!

.. block-info:: Source code and documentation

    You can find further information and source code of this example
    :dox:`in the documentation <examples-picking>`.
