## NPM and package.json

The <code>package.json</code> file is standard to all Node.js modules using [NPM](http://npmjs.org/) (which implements the [CommonJS package format](http://wiki.commonjs.org/wiki/Packages/1.1) specification.

Without going into too much detail, here's what you need to know about each.

<p><a name="packagejson" href="#packagejson"></a></p>

<h2><code>package.json</code></h2>

<p>The <code>package.json</code> file is a JSON description of your project.  This is the default <code>package.json</code> for a generated Tower.js app:</p>

<div class="highlight"><pre lang=" json">{
  "name":                 "my-app",
  "version":              "0.0.1",
  "description":          "Some one-liner description of your project.",
  "homepage":             "<a href='http://github.com/username/my-app'>http://github.com/username/my-app</a>",
  "main":                 "./server.js",
  "author":               "Your Name <<a href='mailto:your@email.com'>your@email.com</a>>",
  "keywords": [
    "node"
  ],
  "maintainers": [{
    "name":               "Your Name",
    "email":              "<a href='mailto:your@email.com'>your@email.com</a>"
  }],
  "contributors": [

  ],
  "licenses": [
    {
      "type":             "MIT",
      "url":              "<a href='http://mths.be/mit'>http://mths.be/mit</a>"
    }
  ],
  "bugs": {
    "url":                "<a href='http://github.com/username/my-app/issues'>http://github.com/username/my-app/issues</a>"
  },
  "repository": {
    "type":               "git",
    "url":                "<a href='http://github.com/username/my-app.git'>http://github.com/username/my-app.git</a>"
  },
  "engines": {
    "node":               ">= 0.4.0"
  },
  "dependencies": {
    "tower":              ">= 0.3.0"
  },
  "devDependencies": {
    "coffee-script":      ">= 1.1.3",
    "stylus":             ">= 0.17.0",
    "uglify-js":          ">= 1.1.1",
    "design.io":          ">= 0.1.0"
  }
}
</pre></div>

<p>For all node modules that you'll need in both development and production environments, put them into <code>dependencies</code>.  Anything that you just need for development (like <code>uglify-js</code> for javascript compression), put that into <code>devDependencies</code>.</p>

<p>To install just production dependencies, run this command from your project root:</p>

<div class="highlight"><pre lang="">npm install --production
</pre></div>

<p>You can also do this by setting <code>NODE_ENV</code> to <code>production</code>.</p>

<p>To install both <code>dependencies</code> and <code>devDependencies</code>, run this command:</p>

<div class="highlight"><pre lang="">npm install
</pre></div>

<p><strong>Note</strong>: When you push your app to Heroku, it's compiled to a "slug".  This makes it easy to clone and scale your app.  The ideal is to minimize the size of that slug so it's faster to clone.  The two biggest ways to do this are 1) minimize the number of images/pdfs/assets in your project (put them on S3 or something), and 2) minimize the number of modules you use in production.  So if you only need gzipping to compile your assets for production, put that into your <code>devDependencies</code>.</p>

<p><a name="npm" href="#npm"></a></p>

<h2>NPM</h2>

<p>Most of you are probably familiar with NPM, but here's just a quick list of helpful commands to streamline your development workflow.</p>

<p>More on NPM here: <a href='http://npmjs.org/doc'>http://npmjs.org/doc</a>.</p>

<p><a name="npm-install" href="#npm-install"></a></p>

<h3><code>npm install</code></h3>

<p>The first thing to do when you create a Tower project (or you <code>git clone</code> any node module from GitHub) is to <code>cd</code> into the project root and install the dependencies:</p>

<div class="highlight"><pre lang="">npm install
</pre></div>

<p>You can install a published npm module like this:</p>

<div class="highlight"><pre lang="">npm install coffee-script jade stylus
</pre></div>

<p>You can also install local modules:</p>

<div class="highlight"><pre lang="">npm install /Users/username/git/node/my-module
</pre></div>

<p>But when you're developing a module locally, it's a pain to constantly have to remove and reinstall it into the app that's using it.  That's where <code>npm link</code> comes in.</p>

<p><a name="npm-link" href="#npm-link"></a></p>

<h3><code>npm link</code></h3>

<p>The <code>npm link</code> command creates a symlink to a local node module that links it to the global npm module directory on your hard drive.  First step is to link <code>my-module</code> to the global directory:</p>

<div class="highlight"><pre lang="">$ cd /Users/username/git/node/my-module
$ npm link
/usr/local/lib/node_modules/my-module -> /Users/username/git/node/my-module
</pre></div>

<p>Then in any local project that's using it, run:</p>

<div class="highlight"><pre lang="">$ npm link my-module
./node_modules/my-module -> /usr/local/lib/node_modules/my-module -> /Users/username/git/node/my-module
</pre></div>

<p>I love <code>npm link</code>, it makes development so much easier.</p>

<p><a name="npm-publish" href="#npm-publish"></a></p>

<h3><code>npm publish</code></h3>

<p>To publish a module to the npm index, run:</p>

<div class="highlight"><pre lang="">npm publish
</pre></div>

<p><a name="npm-scripts" href="#npm-scripts"></a></p>

<h3>NPM Scripts</h3>

<p>Sometimes you want to run some code before/after your module is installed. Do this by defining npm <code>scripts</code> in your <code>package.json</code> (<a href='http://npmjs.org/doc/scripts.html'>http://npmjs.org/doc/scripts.html</a>).</p>

<p>Here's a <code>package.json</code> that installs a RubyGem after the node module is installed:</p>

<div class="highlight"><pre lang=" json">{
  ...
  "scripts": {
    "install": "gem install nokogiri"
  }
}
</pre></div>

<p>Run custom scripts like this:</p>

<div class="highlight"><pre lang="">npm run-script docs
</pre></div>

<p><a name="references" href="#references"></a></p>

<h2>References</h2>

<ul>
<li><a href='http://npmjs.org/doc/json.html'>http://npmjs.org/doc/json.html</a></li>
<li><a href='http://npmjs.org/doc/config.html'>http://npmjs.org/doc/config.html</a></li>
</ul>

<h1>Application Tasks</h1>

<p><a name="cake-assets-bundle" href="#cake-assets-bundle"></a></p>

<h2><code>cake assets:bundle</code></h2>

<p><a name="cake-assets-upload" href="#cake-assets-upload"></a></p>

<h2><code>cake assets:upload</code></h2>

<p><a name="cake-assets-stats" href="#cake-assets-stats"></a></p>

<h2><code>cake assets:stats</code></h2>

<p><a name="cake-routes" href="#cake-routes"></a></p>

<h2><code>cake routes</code></h2>

<p><a name="cake-docs" href="#cake-docs"></a></p>

<h2><code>cake docs</code></h2>

<p><a name="npm-test" href="#npm-test"></a></p>

<h2><code>npm test</code></h2>

<h2>Other useful commands</h2>

<div class="highlight"><pre lang="">

<h1><a href='https://npmjs.org/doc/config.html'>https://npmjs.org/doc/config.html</a></h1>

$ npm config get browser
open
$ npm config get editor
vi
$ npm config get shell
/bin/bash
</pre></div>