Sikuli In a Container
=====================

This container should allow you to quickly and easily get Sikuli working on any
Linux system that is a Docker host.

    docker run -it --rm -v $(pwd):/code -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=unix$DISPLAY codekoala/sikuli

In this command, we're mapping the current directory to ``/code`` in the
container. This is how we can share our existing Sikuli tests with a new
container, or save new tests that can be reused later.

Next, we have to mount our X11 socket inside of our container so Sikuli can
access our existing X11 session. Along with that, we set an environment
variable (``DISPLAY``) to point to this socket in the container.

If you simply wish to run an existing Sikuli script, use the command above and
add a ``-r relative/path/to/test.sikuli``.

    docker run -it --rm -v $(pwd):/code -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=unix$DISPLAY codekoala/sikuli -r test.sikuli

Simplify
--------

You can simplify this quite a bit if you turn this into an alias. For example:

    alias sikuli='docker run -it --rm -v `pwd`:/code -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=unix$DISPLAY codekoala/sikuli '

You can put this alias in your ``~/.bashrc`` or ``~/.zshrc``, depending on
which shell you use. Note the use of single quotes instead of double quotes, as
well as the trailing space inside of the alias definition. The single quotes
will make it so things like ``pwd`` are evaluated when the alias is invoked
rather than when it's defined. The space at the end allows us to pass
additional arguments to our container, such as ``sikuli -r test.sikuli``.

Troubleshooting
---------------

If you get errors about not being able to connect to an X11 session, you may
need to run something like ``xhost +`` on your Docker host.
