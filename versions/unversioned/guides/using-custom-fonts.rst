.. _using-custom-fonts:

******************
Using Custom Fonts
******************

Both iOS and Android come with their own set of platform fonts but if you want
to inject some more brand personality into your app, a well picked font can go
a long way. In this guide we'll walk you through adding a custom font to your
Exponent app. We'll use `Open Sans
<https://fonts.google.com/specimen/Open+Sans>`_ from `Google Fonts
<https://fonts.google.com/>`_ in the example, and the process is identical for
any other font, so feel free to adapt it to your use case. Before proceeding,
go ahead and download `Open Sans
<https://fonts.google.com/specimen/Open+Sans>`_

Starting code
=============

First let's start with a basic "Hello world!" app. Create a new project in XDE/exp and change
``main.js`` to the following:

.. code-block:: javascript

  import {
    Text,
    View,
  } from 'react-native';

  import React from 'react';
  import Exponent from 'exponent';

  class App extends React.Component {
    render() {
      return (
        <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
          <Text style={{ fontSize: 56 }}>
            Hello, world!
          </Text>
        </View>
      );
    }
  }

  Exponent.registerRootComponent(App);

Try getting this basic app running before playing with Open Sans, so you can
get any basic setup issues out of the way.

Downloading the font
====================

Take the Open Sans zipfile that you downloaded, extract it and copy
``OpenSans-Bold.ttf`` into the assets directory in your project. The location
we recommend is ``your-project/assets/fonts``.

.. .. epigraph::
..   **Note:** We don't *have to* download the font, we could alternatively load it from the web. We recommend it, though, so that it doesn't just disappear on you like things on the web sometimes do.

Loading the font in your app
============================

To load and use fonts we will use the :ref:`Exponent SDK <exponent-sdk>`, which
comes pre-installed when you create a new Exponent project, but if for some
reason you don't have it, you can install with ``npm install --save exponent``
in your project directory. Add the following ``import`` in your application
code:

.. code-block:: javascript

   import { Font } from 'exponent';

The ``exponent`` library provides an API to access native functionality of the
device from your JavaScript code. ``Font`` is the module that deals with
font-related tasks. First, we must load the font from our assets directory using
:func:`Exponent.Font.loadAsync`. We can do this in the `componentDidMount()
<https://facebook.github.io/react/docs/component-specs.html#mounting-componentdidmount>`_
lifecycle method of the ``App`` component. Add the following method in ``App``:
Now that we have the font files saved to disk and the Font SDK imported, let's
add this code:

.. code-block:: javascript

      class App extends React.Component {
        componentDidMount() {
          Font.loadAsync({
            'open-sans-bold': require('./assets/fonts/OpenSans-Bold.ttf'),
          });
        }

        // ...
      }

This loads Font Awesome and associates it with the name ``'open-sans-bold'`` in
Exponent's font map. Now we just have to refer to this font in our ``Text``
component.

.. epigraph::
  **Note:** Fonts loaded through Exponent don't currently support the ``fontWeight`` or ``fontStyle`` properties -- you will need to load those variations of the font and specify them by name, as we have done here with bold.

Using the font in a ``Text`` component
======================================

With React Native you specify fonts in ``Text`` components using the
``fontFamily`` style property. The ``fontFamily`` is the key that we used
with ``Font.loadAsync``.

.. code-block:: javascript

      <Text style={{ fontFamily: 'open-sans-bold', fontSize: 56 }}>
        Hello, world!
      </Text>

On next refresh the app seems to still not display the text with Open Sans Bold.
You will see that it is still using the default system font. The problem is that
:func:`Exponent.Font.loadAsync` is an asynchronous call and takes some time to
complete. Before it completes, the ``Text`` component is already rendered with
the default font since it can't find the ``'open-sans-bold'`` font (which hasn't been
loaded yet).

Waiting for the font to load before rendering
=============================================

We need a way to re-render the ``Text`` component when the font has finished
loading. We can do this by keeping a boolean value ``fontLoaded`` in the ``App``
component's state that keeps track of whether the font has been loaded. We
render the ``Text`` component only if ``fontLoaded`` is ``true``.

First we initialize ``fontLoaded`` to false in the ``App`` class constructor:

.. code-block:: javascript

    class App extends React.Component {
      state = {
        fontLoaded: false,
      };

      // ...
    }

Next, we must set ``fontLoaded`` to ``true`` when the font is done loading.
:func:`Exponent.Font.loadAsync` returns a ``Promise`` that is fulfilled when the
font is successfully loaded and ready to use. So we can use `async/await <https://blog.getexponent.com/react-native-meets-async-functions-3e6f81111173>`_
with ``componentDidMount()`` to wait until the font is loaded, then update our state.

.. code-block:: javascript

      class App extends React.Component {
        async componentDidMount() {
          await Font.loadAsync({
            'open-sans-bold': require('./assets/fonts/OpenSans-Bold.ttf'),
          });

          this.setState({ fontLoaded: true });
        }

        // ...
      }

Finally, we want to only render the ``Text`` component if ``fontLoaded`` is
``true``. We can do this by replacing the ``Text`` element with the following:

.. code-block:: javascript

          <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
            {
              this.state.fontLoaded ? (
                <Text style={{ fontFamily: 'open-sans-bold', fontSize: 56 }}>
                  Hello, world!
                </Text>
              ) : null
            }
          </View>

A ``null`` child element is simply ignored by React Native, so this skips
rendering the ``Text`` component when ``fontLoaded`` is ``false``. Now on
refreshing the app you will see that ``open-sans-bold`` is used.

This technique is built into the Tabs template for convenience, as you can see
`here <https://github.com/exponentjs/new-project-template/blob/2fdf49b77090124c317e4eb73684f6bae782a4fd/main.js#L30-L50>`_.

.. epigraph::
  **Note:** Typically you will want to load your apps primary fonts before the app is displayed to avoid text flashing in after the font loads. The recommended approach is to move the ``Font.loadAsync`` call to your top-level component.
