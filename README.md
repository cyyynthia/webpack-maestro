# Webpack Maestro
[![ko-fi](https://www.ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/G2G71TSDF)<br>
[![License](https://img.shields.io/github/license/cyyynthia/webpack-maestro.svg?style=flat-square)](https://github.com/cyyynthia/webpack-maestro/blob/mistress/LICENSE)

WIP: An experiment to see if it's possible to make shareable webpack configurations, and reduce the pain of the
initial webpack installation and configuration by creating a completely new way of configuring webpack.

## Why?
 - **Webpack is a complex tool**<br>
   Most of webpack configurations end up being extremely large and, for me, hard to maintain and to keep up to
   date.
 - **A simple webpack config requires tons of modules**<br>
   Webpack, as powerful as it is, comes really barebone and can quickly require dozens of additional loaders and
   plugins for a single project.
 - **Shareable configs ftw**<br>
   Push your config once, use anywhere. You can define specific modules and imports only chunk of a really complete
   config (for example, only import React when you're using it, but have it in the shared config).
 - **Fluent APIs are more appealing to me**<br>
   So I made one, instead of a giant object which is hard to make modular.

## How it works?
TBD; I want to ship in:
 - Automatic dependency installation (and addition to package.json)
 - A fluent API to add and remove things from the config super easily
 - A way to share configs in form of npm packages
 - Avoid having to `require` plugins, to make the config file look even prettier
