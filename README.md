Sikuli In a Container
=====================

This container should allow you to quickly and easily get Sikuli working on any
Linux system that is a Docker host.

    docker run -it --rm -u $UID -v $(pwd):/root -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=unix$DISPLAY codekoala/sikuli

In this command, we're mapping the current directory to ``/root`` in the
container. This is how we can share our existing Sikuli tests with a new
container, or save new tests that can be reused later.

Next, we have to mount our X11 socket inside of our container so Sikuli can
access our existing X11 session. Along with that, we set an environment
variable (``DISPLAY``) to point to this socket in the container.

If you simply wish to run an existing Sikuli script, use the command above and
add a ``-r relative/path/to/test.sikuli``.

    docker run -it --rm -u $UID -v $(pwd):/root -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=unix$DISPLAY -w /root codekoala/sikuli -r test.sikuli

Simplify
--------

You can simplify using containerized Sikuli quite a bit if you create some
aliases. For example:

    _dsikuli='docker run -it --rm -u $UID -v `pwd`:/root -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=unix$DISPLAY '
    alias sikuli="$_dsikuli codekoala/sikuli "
    alias sikuli-run="$_dsikuli -w /root codekoala/sikuli -r "

You can put this alias in your ``~/.bashrc`` or ``~/.zshrc``, depending on
which shell you use. Note the use of single quotes versus double quotes, as
well as the trailing space inside of the alias definitions. The single quotes
will make it so things like ``pwd`` are evaluated when the alias is invoked
rather than when it's defined. The space at the end allows us to pass
additional arguments to our container, such as ``sikuli-run test.sikuli``.

Troubleshooting
---------------

If you get errors about not being able to connect to an X11 session, you may
need to run something like ``xhost +`` on your Docker host.
