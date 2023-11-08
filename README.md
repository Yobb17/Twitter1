/* ==UserStyle==
@name           Custom Chrome image background color.
@description    Changes the background color of an image with transparency when viewing it directly.
@version        1.0.2
@author         Zennn
@namespace      zennn
@license        WTFPL
@updateURL      https://gist.github.com/doZennn/e4d706e2db289045f68b133d9c0076e4/raw/custom_chrome_image_background.user.css
@preprocessor   less
@var color bgcolor "Background Color" transparent
==/UserStyle== */

/*
    There's no way to only select images with a userstyle. So apply to all pages then be super specific when selecting the img tag to avoid false positives.
    Ref: https://chromium.googlesource.com/chromium/src/+/refs/heads/master/third_party/blink/renderer/core/html/image_document.cc#409
*/
html[style="height: 100%;"] {
    > body[style="margin: 0px; background: #0e0e0e; height: 100%"],
    > body[style="margin: 0px; height: 100%; background-color: rgb(14, 14, 14);"] {
        > img[style="display: block;-webkit-user-select: none;margin: auto;background-color: hsl(0, 0%, 90%);transition: background-color 300ms;"],
        > img[style="display: block;-webkit-user-select: none;margin: auto;cursor: zoom-in;background-color: hsl(0, 0%, 90%);transition: background-color 300ms;"],
        > img[style="display: block;-webkit-user-select: none;margin: auto;cursor: zoom-out;background-color: hsl(0, 0%, 90%);transition: background-color 300ms;"] {
            background-color: @bgcolor !important;
        }
    }
}
