HPlayground
==========
HPlayground is a subset of [Transient](https://github.com/agocorona/transient) running widgets in the Web browser with the Haste compiler. The widgets are first class, that means that there is an widget algebra using standard haskell combinators.

latest additions:

* [A monad for reactive programming part 2](https://www.fpcomplete.com/user/agocorona/monad-reactive-programming-2) describes the monadic reactive mechanism of HPlayground

* A [Tutorial for creating client side applications](http://www.airpair.com/haskell/posts/haskell-tutorial-introduction-to-web-apps) using Haste and HPlayground

* [ajax](http://tryplayg.herokuapp.com/try/ajax.hs/edit) with de-inversion of control

* [WebSockets](http://tryplayg.herokuapp.com/try/hplay-sockets.hs/edit) with de-inversion of control

* A [accounting application example](http://tryplayg.herokuapp.com/try/mybudget.hs/edit) that shows how to include a javascript library -the google chart library- as a widget.

Create applications in the browser as fast as easyly as console applications and have reactive, window-oriented
and spreadsheet-like behaviours for free.

So you translate your inputs and outputs from console calls to HPlayground widgets and with no more modifications
you have reactive and spreadsheet behaviours.

HPlayground uses the same widgets and combinators used in [MFlow](https://github.com/agocorona/MFLow). MFlow is a server-side framework.

This program creates two input boxes and presents the sum below them:


        import Haste.HPlay.View
        import Control.Applicative


        main= runBody action

        action :: Widget ()
        action = do
             r  <- (+) <$> inputInt Nothing `wake` OnKeyPress <++ br
                       <*> inputInt Nothing `wake` OnKeyPress <++ br
             p  (show r) ++> noWidget

The coming extension  `applicativeDo` for the GHC compiler will make this syntax much easier.
Each widget creates his own rendering and manages his own events, that can be propagated
or not down through the monadic computation and triggers modifications in the DOM.

IDE with EXAMPLES, EXAMPLES and more EXAMPLES
============================================

There is an IDE for Haste and HPlayground with many examples running at:

http://tryplayg.herokuapp.com


You can install this IDE locally or in an Heroku instance. Follow the instructions at:

https://github.com/agocorona/tryhplay


Additionally you can see a more complex example: the [hplay-todo](https://github.com/agocorona/hplay-todo),
 the [todoMVC](http://todomvc.com) project for HPlayground.

The [todo application running](http://mflowdemo.herokuapp.com/todo.html)


How it works
============
Under the hood there is the good old formlet concept. It uses monadic and applicative combinators
the same ones used by MFlow in the server side. While the server side widgets of MFlow
produce blaze-html output converted to bytestrings, HPlayground constructs a builder function that
creates a tree in the HTML DOM when executed. This builder (perch) is monoidal so the formlet
can aggregate subtrees. When some event happens in the widget subtree, the widget executes
its code and reconstructs itself. If it returns a valid result and it is in a monadic computation,
the tree continues recreating itself downstream by executing further widgets in the monadic sequence.
If the event is raised within a widget that does not generate a valid result (return empty),
the remaining widgets continue unchanged and unevaluated.

Status
======

Non-local modifications of the DOM work with the new "at" primitive.
Option buttons, checkboxes and drop-down buttons work with the same syntax as in MFlow.

The Cell module has Lens-like primitives for updating form elements and experimental math
operations with form elements as spreadsheet cells. Currently it is at the beginning.

How to run
----------

### Docker file

https://registry.hub.docker.com/u/agocorona/tryhplay/

contains everything necessary to use HPlayground

execute it as:

> sudo docker run -it -p 80:80 agocorona/tryplayg

it runs the examples IDE at port 80, where you can also create and compile new programs with a web browser.

To access the IDE, in Windows and Mac you can obtain the IP of the docker instance with:

> boot2docker ip

use the IP address as the URL in the browser.

### Install from scratch

install the [ghc compiler](http://www.haskell.org/platform/)

install Haste:

    >cabal install haste-compiler
    >haste-boot
    

clone HPlayground:

    >git clone http://github.com/agocorona/hplayground



install hplayground:

    >haste-inst install

  or install it from Hackage using cabal:

    >haste-inst install hplayground

  it will also install `haste-perch`

compile:

    >cd src
    >hastec Main.hs --output-html

`hastec` uses ghc internally so you can expect ordinary ghc error messages in your development.

Browse the Main.html file. In windows simply execute it in the command line:

    >Main.html

you can also see it executing at

     http://mflowdemo.herokuapp.com/noscript/wiki/browserwidgets

Main.html and Main.js are included in the repo so you can execute it in your PC.

Execute it in the same directory where Main.js is, since it references it assuming that it is in the current folder.h

