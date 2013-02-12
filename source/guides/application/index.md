## Tower Application

<h2>Application File Structure</h2>

<div class="highlight"><pre lang="">.
├── app
│   ├── config
│   │   ├── client
│   │   │   ├── bootstrap.coffee
│   │   │   └── watch.coffee
│   │   ├── server
│   │   │   ├── environments
│   │   │   │   ├── development.coffee
│   │   │   │   ├── production.coffee
│   │   │   │   └── test.coffee
│   │   │   ├── initializers
│   │   │   ├── assets.coffee
│   │   │   ├── bootstrap.coffee
│   │   │   ├── credentials.coffee
│   │   │   ├── databases.coffee
│   │   │   └── session.coffee
│   │   └── shared
│   │       ├── locales
│   │       │   └── en.coffee
│   │       ├── application.coffee
│   │       └── routes.coffee
│   ├── controllers
│   │   ├── client
│   │   │   ├── applicationController.coffee
│   │   │   └── postsController.coffee
│   │   └── server
│   │       ├── applicationController.coffee
│   │       └── postsController.coffee
│   ├── models
│   │   ├── client
│   │   ├── server
│   │   └── shared
│   │       └── post.coffee
│   ├── stylesheets
│   │   ├── client
│   │   │   └── application.styl
│   │   └── server
│   │       └── email.styl
│   ├── templates
│   │   ├── server
│   │   │   └── layout
│   │   │       ├── _meta.coffee
│   │   │       └── application.coffee
│   │   └── shared
│   │       ├── layout
│   │       │   ├── _body.coffee
│   │       │   ├── _flash.coffee
│   │       │   ├── _footer.coffee
│   │       │   ├── _header.coffee
│   │       │   ├── _navigation.coffee
│   │       │   └── _sidebar.coffee
│   │       ├── posts
│   │       │   ├── _flash.coffee
│   │       │   ├── _form.coffee
│   │       │   ├── _item.coffee
│   │       │   ├── _list.coffee
│   │       │   ├── _table.coffee
│   │       │   ├── edit.coffee
│   │       │   ├── index.coffee
│   │       │   ├── new.coffee
│   │       │   └── show.coffee
│   │       └── welcome.coffee
│   └── views
│       └── client
│           ├── layout
│           │   └── application.coffee
│           └── posts
│               ├── form.coffee
│               ├── index.coffee
│               └── show.coffee
├── data
│   └── seeds.coffee
├── lib
├── log
├── public
│   ├── fonts
│   ├── images
│   ├── javascripts
│   ├── stylesheets
│   ├── swfs
│   ├── uploads
│   ├── 404.html
│   ├── 500.html
│   ├── favicon.ico
│   ├── humans.txt
│   └── robots.txt
├── scripts
│   └── tower
├── test
│   ├── cases
│   │   ├── controllers
│   │   │   ├── client
│   │   │   └── server
│   │           └── postsControllerTest.coffee
│   │   ├── features
│   │   │   └── client
│   │   └── models
│   │       ├── client
│   │       ├── server
│   │       └── shared
│   │           └── postTest.coffee
│   ├── factories
│   │   └── postFactory.coffee
│   ├── client.coffee
│   ├── mocha.opts
│   └── server.coffee
├── tmp
├── wiki
│   ├── _sidebar.md
│   └── home.md
├── Cakefile
├── grunt.coffee
├── package.json
├── Procfile
├── README.md
└── server.js
</pre></div>

<p><a name="the-application-object" href="#the-application-object"></a></p>

<h2>The Application Object</h2>

<p>The main application is defined in <code>config/application.coffee</code> and defaults to this:</p>

<div class="highlight"><pre lang=" coffeescript" class="prettyprint language-javascript">class App extends Tower.Application
  @configure ->
    @use "favicon", Tower.publicPath + "/favicon.ico"
    @use "static",  Tower.publicPath, maxAge: Tower.publicCacheDuration
    @use "profiler" if Tower.env != "production"
    @use "logger"
    @use "query"
    @use "cookieParser", Tower.session.secret
    @use "session", Tower.session.key
    @use "bodyParser"
    @use "csrf"
    @use "methodOverride", "_method"
    @use Tower.Middleware.Agent
    @use Tower.Middleware.Location
    @use Tower.Middleware.Router
</pre></div>

<p>The <code>configure</code> function is used to configure the application.</p>

<ul>
<li><code>favicon</code> points to the icon file displayed in the browser address bar.</li>
<li><code>static</code> is used to set where public (static) files reside and define their cache duration on the client.</li>
<li><code>profiler</code> is by default configured to only used only when the app is not in production mode (environment).</li>
</ul>

<p>You can of course extend the configure function with your own application configuration logic as needed.</p>

<p><a name="the-client" href="#the-client"></a></p>

<h3>The Client</h3>

<p>The client application is defined exactly like the server application, they just tend to require different middleware and slightly different configuration.</p>

<p><a name="auto-reloading-changed-files" href="#auto-reloading-changed-files"></a></p>

<h2>Auto-reloading Changed Files</h2>

<p>Internally Tower.js knows when a file was changed.  So when you refresh <a href='http://localhost:3000'>http://localhost:3000</a>, it passes through the <code>Tower.Middleware.Dependencies</code> which re-requires any file that has changed.  This makes development uber fast, and prevents you from having to restart the server whenever a file changes (i.e. <a href="https://github.com/remy/nodemon">nodemon</a>).  I mean, you can use nodemon if you want, it's a great project.  But you don't need to.</p>

<p><a name="configuration" href="#configuration"></a></p>

<h2>Configuration</h2>

<p><a name="environments" href="#environments"></a></p>

<h2>Environments</h2>

<p>Tower by default comes with the following environments:</p>

<ul>
<li>development</li>
<li>test</li>
<li>production</li>
</ul>

<p>Additional environments can be defined if needed (such as staging).</p>

<p><a name="internationalization-i18n" href="#internationalization-i18n"></a></p>

<h2>Internationalization (I18n)</h2>

<div class="highlight"><pre lang=" coffeescript">

<h1>app/config/shared/locales/en.coffee</h1>

module.exports =
  hello: 'world'
  forms:
    titles:
      signup: 'Signup'
  pages:
    titles:
      home: 'Welcome to %{site}'
  posts:
    comments:
      none: 'No comments'
      one: '1 comment'
      other: '%{count} comments'
  messages:
    past:
      none: 'You never had any messages'
      one: 'You had 1 message'
      other: 'You had %{count} messages'
    present:
      one: 'You have 1 message'
    future:
      one: 'You might have 1 message'
</pre></div>

<p>Tower also has some more internal locale files for system generated text such as dates and times etc.<br />You can provide your own locale files for these as well (see the Tower src code and look for /locale folders).  </p>

<p><a name="gruntfile" href="#gruntfile"></a></p>

<h2>Gruntfile</h2>

<ul>
<li>Automatically compile assets when they are added, saved, or removed.  See the <a href="/assets/pipeline">Tower Asset Pipeline</a></li>
<li>Automatically restart the server in development mode when you modify a file</li>
<li>Push compiled CSS and JavaScripts to the browser as you're developing without refreshing the browser!</li>
<li>Automatically run tests across browsers, platforms, and devices as your developing.</li>
</ul>

<p><a name="dotfiles" href="#dotfiles"></a></p>

<h2>Dotfiles</h2>

<p><a href="https://github.com/search?q=dotfiles&amp;type=Everything&amp;repo=&amp;langOverride=&amp;start_value=1">Dotfiles</a> are for the most part just hidden files that different platforms use to store configuration data about your project.</p>

<p>In Tower.js, there's 3 by default:</p>

<ul>
<li><code>.gitignore</code> Tells git the files and directories it should ignore.</li>
<li><code>.npmignore</code> Tells npm what to ignore when your module is published.</li>
<li><code>.slugignore</code> Tells Heroku what to ignore about your project.</li>
</ul>