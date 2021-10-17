---
title: "Firefox Tab Address Bar Font Size"
created: 2021-07-02 13:42:59
---

tags: #howto
keywords: font, firefox

# Firefox Tab Address Bar Font Size

[Change "tab" font size for older users of firefox](https://support.mozilla.org/en-US/questions/1302318)

You can set `layout.css.devPixelsPerPx` to 1.0 (default is -1) on the `about:config` page. Adjust its value in 0.1 or 0.05 steps (1.1 or 0.9) until icons or text looks right.

> modifying layout.css.devPixelsPerPx affects user interface and web pages (global zoom)

You can open the about:config page via the location/address bar. You can accept the warning and click "I accept the risk!" to continue.

    https://support.mozilla.org/en-US/kb/about-config-editor-firefox

Firefox has a Zoom section in Options/Preferences to set the default zoom level for web pages in case you need to compensate for the adjustment of layout.css.devPixelsPerPx.

`Options/Preferences -> General -> Language and Appearance -> Zoom`
