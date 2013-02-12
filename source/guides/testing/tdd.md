## Test Driven Development (TDD)

Test Driven Development (TDD) is a quality of all great apps.  Once you master it you're able to not only develop a lot quicker, what you develop is also built on a much stronger foundation and will be more resilient in the face of change.

The problem with TDD, or BDD (Behavior Driven Development) even, is it's hard to get started with.  You could spend weeks if not months setting up a solid workflow for building your app around tests.  That's why Tower aims to make testing easy and clear.

Tower uses these three libraries for testing:

<ul>
<li><a href="https://github.com/visionmedia/mocha">mocha</a>: The async TDD/BDD framework</li>
<li><a href="https://github.com/logicalparadox/chai">chai</a>: The assertion code to use within mocha tests</li>
<li><a href="https://github.com/cjohansen/Sinon.JS">sinon</a>: A spy/stub/mock framework for testing more advanced/complex code</li>
</ul>

<p><a name="build-testable-objects" href="#build-testable-objects"></a></p>

<h2>Build Testable Objects</h2>

<p>If your system is at all complex, break out functionality into smaller objects that you can test independently.  This is why I broke down Tower "chainable queries" into a <code>Scope</code> and a <code>Criteria</code> object, rather than just having a <code>Scope</code> object.  And the reason why there's a distinction between the "model" and "data" (store/datastore) layers.</p>

<p>By doing this, I was able to quickly build out most of the functionality for query generation using just the <code>Criteria</code> object.  Then I knew I could get back a hash of conditions and options to use in finding records from a data store.</p>

<p>This makes the whole project more manageable and makes rapid development possible.  I found my mind spinning in circles trying to grasp the whole system at once otherwise.</p>

<p>However, note this: there's a tradeoff between testability and performance.  If every single thing was built as nested objects like the above two examples, you'd end up creating a ton of objects to do (often) pretty simple things, and this is bad from both a memory and performance standpoint.  In most cases it also increases the amount of code you have to write, which more than anything means the browser will have to load more code.  So, if something's easy to test without refactoring it into objects, just leave it that way.  It's less code for the browser to download, and will most likely use up less memory, which means a better user experience in the end.</p>

<p><a name="code-coverage" href="#code-coverage"></a></p>

<h2>Code Coverage</h2>

<p><a name="testing-commands-in-towerjs" href="#testing-commands-in-towerjs"></a></p>

<h2>Testing Commands in Tower.js</h2>

<p><a name="testing-controllers-in-towerjs" href="#testing-controllers-in-towerjs"></a></p>

<h2>Testing Controllers in Tower.js</h2>

<p><a name="testing-file-i-o-in-towerjs" href="#testing-file-i-o-in-towerjs"></a></p>

<h2>Testing File I/O in Tower.js</h2>

<p><a name="testing-web-sockets-in-towerjs" href="#testing-web-sockets-in-towerjs"></a></p>

<h2>Testing Web Sockets in Tower.js</h2>

<p><a name="towerfactory" href="#towerfactory"></a></p>

<h2><code>Tower.Factory</code></h2>

<div class="highlight"><pre lang=" coffeescript">Tower.Factory.define 'user', ->
  email: Faker.Email
</pre></div>

<p><a name="fakers" href="#fakers"></a></p>

<h2>Fakers</h2>

<p>Fakers are used to generate random data.</p>

<ul>
<li><a href="https://github.com/marak/Faker.js/">Faker.js</a></li>
<li><a href="http://thechangelog.com/post/607075727/faker-js-generate-fake-data-in-node-js-or-in-your">Faker.js screencast</a></li>
</ul>

<div class="highlight"><pre lang=" coffeescript">Faker       = require 'Faker'

randomName  = Faker.Name.findName() # Rowan Nikolaus
randomEmail = Faker.Internet.email() # <a href='mailto:Kassandra.Haley@erich.biz'>Kassandra.Haley@erich.biz</a>
randomCard  = Faker.Helpers.createCard() # random contact card containing many properties
</pre></div>

<p><a name="testing-with-mocha" href="#testing-with-mocha"></a></p>

<h2>Testing with Mocha</h2>

<p>This command will watch the <code>test</code> directory, and anytime you save a file it will execute the tests.</p>

<div class="highlight"><pre lang="">mocha $(find test -name "*Test.coffee")
</pre></div>

<p>You can run it through the node debugger using the <code>-d</code> option:</p>

<div class="highlight"><pre lang="">mocha $(find test -name "*Test.coffee") -d
</pre></div>

<p><a name="testing-models-in-towerjs" href="#testing-models-in-towerjs"></a></p>

<h2>Testing Models in Tower.js</h2>

<p><a name="example" href="#example"></a></p>

<h3>Example</h3>

<div class="highlight"><pre lang=" coffeescript">

<h1>./test/models/userTest.coffee</h1>

describe "App.User", ->
  user = null

  describe "#fields", ->
    beforeEach (done) ->
      App.User.destroy =>
        user = new App.User

<pre><code>test ".email", -&gt;
  assert.ok user.has("email")
</code></pre>

</pre></div>

<p><a name="testing-views-in-towerjs" href="#testing-views-in-towerjs"></a></p>

<h2>Testing Views in Tower.js</h2>

<p><a href="http://visionmedia.github.com/mocha/">Mocha</a> can be used to "spec" an app including specs for acceptance testing.<br />See the Tower test suite for how to use Mocha to test a Tower app.</p>

<ul>
<li><a href="http://www.adomokos.com/2012/01/javascript-testing-with-mocha.html">Javascript testing with Mocha</a></li>
</ul>

<p>The "BDD" interface provides: describe(), it(), before(), after(), beforeEach(), and afterEach().<br />The "TDD" interface provides: suite(), test(), setup(), and teardown().</p>

<p>To setup Mocha for browser use all you have to do is include the script, stylesheet, tell Mocha which interface you wish to use, and then run the tests. A typical setup might look something like the following, where we call <code>mocha.setup('bdd')</code> to use the <em>BDD</em> interface before loading the test scripts, running them onload with <code>mocha.run()</code>.</p>

<div class="highlight"><pre lang=" html"><html>
<head>
  <meta charset="utf-8">
  <title>Mocha Tests</title>
  <link rel="stylesheet" href="mocha.css" />
  <script src="jquery.js"></script>
  <script src="expect.js"></script>
  <script src="mocha.js"></script>
  <script>mocha.setup('bdd')</script>
  <script src="test.array.js"></script>
  <script src="test.object.js"></script>
  <script src="test.xhr.js"></script>
  <script>
    $(function(){
      mocha
        .globals(['foo', 'bar']) // acceptable globals
        .run()
    })
  </script>
</head>
<body>
  <div id="mocha"></div>

<p></body><br /></html><br /></pre></div></p>

<p><a name="acceptance-testing" href="#acceptance-testing"></a></p>

<h2>Acceptance Testing</h2>

<p>Use the Mocha BDD interface for Acceptance Testing. The Acceptance Tests can be executed from the console or from within the browser.</p>

<ul>
<li><a href='https://browserlab.adobe.com/en-us/index.html'>https://browserlab.adobe.com/en-us/index.html</a></li>
<li><a href='http://browsershots.org/'>http://browsershots.org/</a></li>
<li><a href='https://browserling.com'>https://browserling.com</a></li>
<li><a href='http://www.browserstack.com/test-in-internet-explorer'>http://www.browserstack.com/test-in-internet-explorer</a></li>
<li><a href='https://saucelabs.com/'>https://saucelabs.com/</a></li>
<li><a href='http://www.smashingmagazine.com/2011/08/07/a-dozen-cross-browser-testing-tools/'>http://www.smashingmagazine.com/2011/08/07/a-dozen-cross-browser-testing-tools/</a></li>
</ul>

<p><a name="resources" href="#resources"></a></p>

<h2>Resources</h2>

<ul>
<li><a href='https://github.com/visionmedia/node-jscoverage'>https://github.com/visionmedia/node-jscoverage</a></li>
<li><a href='https://github.com/viatropos/design.io'>https://github.com/viatropos/design.io</a></li>
</ul>