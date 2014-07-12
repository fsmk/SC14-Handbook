

![Introduction to Node.js][1]

Introduction to Node.js
======

Node.js is a runtime environment and a library for running applications written in JavaScript outside the browser (for example, on the server).

Node.js applications are designed to maximize throughput and efficiency, using non-blocking I/O and asynchronous events. Node.js applications run single-threaded, although Node.js uses multiple threads for file and network events. Node.js is commonly used for real time applications due to its asynchronous nature, allowing applications to display information faster for users without the need for refreshing.

## Development ##
Node.js has a developer community primarily centered on two mailing lists and the IRC channel #node.js on freenode. The community gathers at NodeConf, an annual developer conference focused on Node.js.
Node.js is currently used by a number of large companies including Groupon, SAP,LinkedIn,Microsoft,Yahoo!,Walmart and PayPal.


Server-side JavaScript
----------------------

The first incarnations of JavaScript lived in browsers. But this is just the context. It defines what you can do with the language, but it doesn't say much about what the language itself can do. JavaScript is a "complete" language: you can use it in many contexts and achieve everything with it you can achieve with any other "complete" language.

Node.js really is just another context: it allows you to run JavaScript code in the backend, outside a browser.

In order to execute the JavaScript you intend to run in the backend, it needs to be interpreted and, well, executed. This is what Node.js does, by making use of Google's V8 VM, the same runtime environment for JavaScript that Google Chrome uses.

Plus, Node.js ships with a lot of useful modules, so you don't have to write everything from scratch, like for example something that outputs a string on the console.

Thus, Node.js is really two things: a runtime environment and a library.

In order to make use of these, you need to install Node.js. Instead of repeating the process here, I kindly ask you to visit the official installation instructions. Please come back once you are up and running.


Intstalling Node js
--------------------------------
Install the dependencies:

sudo apt-get install g++ curl libssl-dev apache2-utils
sudo apt-get install git-core

Run the following commands:

    ubuntu_setup.sh
    git clone git://github.com/ry/node.git
    cd node
    ./configure
    make
    sudo make install

You can refer [Building and installing Node.js][2]


To test if it works run the hello world program
Open your favorite editor and create a file called helloworld.js. We want it to write "Hello World" to STDOUT, and here is the code needed to do that:`

    console.log("Hello World");

Save the file, and execute it through Node.js:

    node helloworld.js
    
    

A hello world http server
-------------------------

Now printing hello world to a terminal isn't all that exciting. Let's take the next step and write a program that responds to hello world via http. We'll call the file 'hello_http.js' and put the following code into it:

     var http = require('http');
     var server = http.createServer(function(req, res) {
     res.writeHead(200);
     res.end('Hello Http');
     });
     server.listen(8080);

Now run this program from the terminal by typing:

    $ node hello_http.js
The first thing you'll notice is that this program, unlike our first one, doesn't exit right away. That's because a node program will always run until it's certain that no further events are possible. In this case the open http server is the source of events that will keep things going.

Testing the server is as simple as opening a new browser tab, and navigating to the following url: http://localhost:8080/. As expected, you should see a response that reads: 'Hello Http'.

Alternatively, you could also open up a new terminal and use curl to test your server:

    $ curl localhost:8080
Hello Http

Introduction to npm
-------------------

Installing npm
Installation of node.js should install npm.
You can check if npm is installed by 
Checking npm version

npm -v
Outputs

1.4.3[Or whcihever latest version is available]
If it is not installed refer the link [introduction to npm][3]


Frameworks
----------

Express

At this point express is probably the go-to framework for most node.js developers. It's relatively mature, and includes the connect (think rack) middleware layer. Included in the package are routing, configuration, a template engine, POST parsing and many other features.

While express is a solid framework, it's currently addressing a much smaller scope than fullstack frameworks like Rails, CakePHP or Django. It's more comparable to Sinatra, and unfortunately doesn't really make a big effort to differentiate itself from its Ruby roots into something that feels natural in JavaScript. Anyhow, short of writing your own framework, it's certainly a great choice at this point.

Resources
---------
Videos
[Crackford videos][4]
[Lynda videos][5]
Books
[Professional node.js][6]
[Learning node][7]
Tutorials
[Codeschool][8]
Mailing list
[Node.js_mailing_list][9]



  References
 [Wikipedia][10]
 [Nodeguide][11]
 [Beginning node][12]


  [1]: http://www.kitware.com/blog/files/6_1529600465.png
  [2]: https://github.com/joyent/node/wiki/installation
  [3]: http://howtonode.org/introduction-to-npm
  [4]: http://yuiblog.com/crockford/
  [5]: http://www.lynda.com/Nodejs-tutorials/Nodejs-First-Look/101554-2.html
  [6]: it-ebooks.info/book/984/
  [7]: http://it-ebooks.info/book/830/
  [8]: http://node.codeschool.com/levels/1
  [9]: https://groups.google.com/forum/#!forum/nodejs
  [10]: http://en.wikipedia.org/wiki/Node.js
  [11]: http://nodeguide.com/beginner.html
  [12]: http://www.nodebeginner.org/
