.. _using-clojurescript:

============================================
Using Clojurescript
============================================

.. image:: img/cljs.png
    :width: 200

.. epigraph::
  **TLDR:** Codes below will create an Exponent project, and connect you to a stable Clojurescript repl.

.. code-block:: bash

   lein new exponent your-project

   cd your-project && npm install

   lein figwheel

   # open a new tab, run
   exp start -i ios

   # it will start the Exponent server and install your app to the iOS simulator

Why Clojurescript
"""""""""""""""""""""""""""""""""""""""""""""""""

- First-class immutable data structures
- Minimizing state and side-effects
- Practicality and pragmatism are always core values of Clojurescript
- Lisp is cool
- Great Javascript interop


Why on Exponent
"""""""""""""""""""""""""""""""""""""""""""""""""

It all begins with a `Simple Made Easy <https://www.infoq.com/presentations/Simple-Made-Easy>`_ design choice: **you don't write native codes**.

- You only write Clojurescript or Javascript.
- You won't be stumbled by Xcode or Android Studio.

  Since you don't need to install them or create corresponding projects at all!
- Reliable upgrading.

  Exponent will take care of upgrading the native modules, React Native versions, you only need to upgrade your Clojurescript or Javascript code.
- You can finally completely develop iOS apps on Linux or Windows.

  Since the Exponent client contains the full native environment.
- It's dead simple to continually share your apps.

  Once you published your app, you got a link. It is up to you to share the link.

1. Create an Exponent project
"""""""""""""""""""""""""""""""""""""""""""""""""

.. code-block:: bash

   # Default to use Reagent / Re-frame
   lein new exponent your-project
   # or Om Next
   lein new exponent your-project +om

   cd your-project && npm install

2. Connect to the REPL
"""""""""""""""""""""""""""""""""""""""""""""""""

We'll show how to do this on CLI, Emacs and `Cursive <https://cursive-ide.com/>`_:

CLI
'''''''''''''''''''''''''''

.. code-block:: bash

   lein figwheel

Emacs
'''''''''''''''''''''''''''

1. Invoke cider-jack-in.
2. Run ``(start-figwheel)`` in the connected REPL.

Cursive
'''''''''''''''''''''''''''

Create a Leiningen nREPL Configuration
---------------------------------------------------

1. Click Run->Edit configurations.
2. Click the + button at the top left and choose Clojure REPL
3. Choose a Local REPL
4. Enter a name in the Name field (e.g. "REPL")
5. Choose the radio button ``Use nREPL with Leiningen``.
6. Click the OK button to save your REPL config.

Connect
---------------------------------------------------

In Intellij make sure your REPL config is selected and click the green **play** button to start your REPL.

Run ``(start-figwheel)`` in the connected REPL.

3. Start Exponent server
"""""""""""""""""""""""""""""""""""""""""""""""""

Using exp CLI
'''''''''''''''''''''''''''

.. code-block:: bash

   # install exp if you haven't
   npm install -g exp

   # connect to iOS simulator
   exp start -i ios

   # or connect to Android devices or simulators
   exp start -i android

For more information, see :ref:`exp Command-Line Interface <exp-cli>`.

Using XDE
'''''''''''''''''''''''''''

For more information, see :ref:`XDE tour <xde-tour>`.

4. Publish your app
"""""""""""""""""""""""""""""""""""""""""""""""""

.. code-block:: bash

   # generate main.js
   lein prod-build

   exp publish

FAQ
"""""""""""""""""""""""""""""""""""""""""""""""""

How do i add custom native modules?
''''''''''''''''''''''''''''''''''''''''''''''''''''''

Currently it's not easy.

For more information, see :ref:`How do I add custom native code to my Exponent project? <faq>`.

Do it support Google Closure advanced compilation?
''''''''''''''''''''''''''''''''''''''''''''''''''''''
It's still experiment, but it already works for multiple projects.

Do it support source maps?
''''''''''''''''''''''''''''''''''''''''''''''''''''''
Yes.

Can I use npm modules?
''''''''''''''''''''''''''''''''''''''''''''''''''''''
React Native uses JavascriptCore, so modules using built-in node like stream, fs, etc wont work. Otherwise, you can just require like: ``(js/require "SomeModule")``.

Do i need to restart repl after adding new Javascript modules or  assets?
'''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''
No, you do need to reload Javascript. To do that, select **Reload** from the Developer Menu.
You can also press ``Command⌘ + R`` in the iOS Simulator, or press ``R`` twice on Android emulators.


Will it support Boot?
'''''''''''''''''''''''''''''

Work in process.
