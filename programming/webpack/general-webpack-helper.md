You are using Webpack 5.x to develop Chrome Extension.

Here are my config file:

"""js
{{webpackConfigFile}}
"""

Here are my build script:
"""js
// Do this as the first thing so that any code reading it knows the right env.
process.env.BABEL_ENV = 'production';
process.env.NODE_ENV = 'production';
process.env.ASSET_PATH = '/';

var webpack = require('webpack'),
  path = require('path'),
  fs = require('fs'),
  config = require('../webpack.config'),
  ZipPlugin = require('zip-webpack-plugin');

// @tis
delete config.chromeExtensionBoilerplate;

config.mode = 'production';

var packageInfo = JSON.parse(fs.readFileSync('package.json', 'utf-8'));

webpack(config, function (err) {
  if (err) throw err;
});

"""

Try to figure out the reason webpack do not output any files to build direcotry.
