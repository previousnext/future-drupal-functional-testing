<!-- Begin assets -->
<link href="./assets/style.css" rel="stylesheet"></link>
<!-- End assets -->

# The Future of Drupal Functional Testing

---

## About the speakers

---

## Nick Schuch

---

## Cameron Zemek

* PHP developer for 10 years
* Drupal/Twitter: grom358

---

## Thanks to the community

![Austin](./assets/austin.jpg "Austin")

Note:
* Thank simpletest, wouldn't have Drupal 8 without it
* Thanks everyone for coming so we can take Drupal testing forwards

---

## What is functional testing

![Unit testing](./assets/unit.jpg "Unit testing") @todo replace graphic

Note:
* Functional testing is a quality assurance (QA) process and a type of black box testing that bases its test cases on the specifications of the software component under test. Functions are tested by feeding them input and examining the output, and internal program structure is rarely considered (not like in white-box testing). Functional Testing usually describes what the system does.
* Functional Testing have many types:
** Smoke Testing - A subset of test cases that cover the most important functionality of a component or system is selected and run, to ascertain if the most crucial functions of a program work correctly.
** Sanity Testing - The point of a sanity test is to rule out certain classes of obviously false results, not to catch every possible error. The advantage of a sanity test, over performing a complete or rigorous test, is speed.
** Regression Testing - is a type of software testing that seeks to uncover new software bugs, or regressions, in existing functional and non-functional areas of a system after changes such as enhancements, patches or configuration changes, have been made to them.
** Usability Testing
* Higher level then unit testing, not dealing with API layer of modules. Working from exposed web interface.
* https://www.drupal.org/node/394888
* Tests are written in such a way that they test the interface as a whole instead of testing individual functions or finite pieces of code.
* Drupal is installed into that environment and the tests are performed. What this means is that the tests always start from the same environment and there is no chance for contamination from other tests. When you write your test it means that you do not have to clean up the environment at the end of each test.

---

## The state of Drupal testing

![Fork](./assets/fork.jpg "Fork")

Note:
* Long ago forked from simpletest (Dries committed it on 21/04/2008)
* Simpletest last release was 23/01/2012
* Maintainers have to maintain this legacy testing system
* PHPUnit already adopted for Unit tests
* KernelTestBase -> Base class for integration tests, can access files and the database, but the entire environment is initially empty.
* WebTestBase. cURL + SimpleXML + custom code
* 

---

## The state of Drupal testing - Cont...

@todo, Diagram of how a site is spun up, tested and torn down.

---

## Time to get off the island

![Island](./assets/island.jpg "Island")

Note:
* BrowserTestBase -> PHPUnit + Mink + custom code

---

## PHPUnit

![Runner](./assets/runner.jpg "Runner")

Note:
* PHPUnit is the standard; most frameworks use it (like Zend Framework (1&2), Cake, Agavi, even Symfony is dropping their own Framework in Symfony 2 for phpunit).
* Already using it in Drupal 8 with unit tests.
* PHPUnit handles running tests, reporting and code coverage.

@todo, diagram of frameworks

---

## PHPUnit example

TODO: Put some demo PHPUnit code in here.

---

## PHPUnit - Contrib

@todo, get links to contrib projects (parallel and UI)

---

## Recognise this syntax

```
Scenario: The content page exists
  Given I am logged in as a user with the "administrator" role
  When I go to "admin/content"
  Then the response status code should be 200
```

Note: Actually powered by Mink

---

## Mink

![Bricks](./assets/bricks.jpg "Bricks")

Note:
* Two types of browser emulators:
** Headless browser emulators (ie. Goutte)
** Browser controllers (ie. Selenium2)
* Headless browsers send a real HTTP requests against an application and parse the response content. Advantages are simplicity, speed and ability to run it without the need in real browser. But this type of browsers have one big disadvantage - they have no JS/AJAX support. So, you can't test your rich GUI web applications with headless browsers.
* Browser controllers simulate user interactions on browser and are able to retrieve actual information from current browser page. Selenium and Sahi are two most famous browser controllers. The main advantage of browser controllers usage is the support for JS/AJAX interactions on page. Disadvantage is that such browser emulators require installed browser, extra configuration are usually much slower than headless counterparts.
* If you choose headless browser emulator - you'll not be able to test your JS/AJAX pages. And if you choose browser controller - your overall test suite will become very slow at some point. So, in real world we should use both! And that's why you need a Mink.
* Mink does the heavy lifting. Remove cURL and form handling code.

---

## Mink example

TODO: Put some demo Mink code in here.

---

## Did someone say JavaScript?

![Javascript](./assets/java.jpg "Javascript")

Note:
* Mouse manipulations: click, doubleClick, rightClick, mouseOver, focus, blur
* Drag & Drop: dragTo
* Not solution to javascript unit testing, but enable full stack testing that includes dynamic frontend
* Handle pages with AJAX

---

## PhantomJS

![Phantom](./assets/phantom.jpg "Phantom")

Note:
* PhantomJS is a headless WebKit scriptable with a JavaScript API. It has fast and native support for various web standards: DOM handling, CSS selector, JSON, Canvas, and SVG.
* Packaged in a binary.

---

## Layers

![I can have my cake and eat it too](./assets/cake.png "Layers")

Note:
* Massive reduction in code in the Browser and Runner layers
* Can use real browsers in the browser layer
* 1000+ lines of custom code implementing a Browser. Replaced with using Mink library.

---

## BrowserTestBase

* How it came to be.
* Mink for browser + PHPUnit for asserts.

---

## Show me the code

WE NEED TO WRITE SOME CODE!

---

## Recommendations

* Replace the Runner/Framework with PHPUnit
* Replace the "Browser" with Mink
* Agree on a Javascript driver for core. (Please come to the BOF!)
* Commit along side existing test suite. Start converting AJAX tests in core.

---

## Blockers

* Testing infrastructure.
** Not easy to swap out run-tests.sh with phpunit
** Hard to setup PhantomJS on testbot
* Please help those guys out! #drupal-infrastructure

---

## How do we get this into Drupal 8 core

Link to patch

---

## Checkout

* Modernizing testbot: The future of drupal.org automated testing (Wednesday 10:45-11:45)
* Automated Frontend testing (Wednesday 14:15-15:15)
* Doing behaviour driven development with behat (Wednesday 15:45-16:45)

---

## BOF

@todo Put our bof session in here.

---

## Question time!
