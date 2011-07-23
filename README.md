Introduction
------------

The IPython Bundle is a collection of commands designed to help you work with IPython from within TextMate. Most commands appear under a menu invoked by the shortcut `⌃⇧I` (Shift + Ctrl + I) when editing Python source code files. Some snippets for scientific Python packages such as SciPy, NumPy and matplotlib are provided as well, as IPython is part of the SciPy project and includes special support for working with these libraries to present a Matlab-like environment for scientific computing.

See the Installation section below if you're viewing this document on the web and have not already installed the bundle.

Please be aware that this bundle currently knows **nothing** of the state of the running IPython. This means that repeated calls may not have the effect you expected. This may change in future versions.


Usage
-----

### Running Code and Viewing State ###

To get started, open or create a Python file, invoke the `⌃⇧I` shortcut and choose `Open IPython` from the menu. This will open a Terminal window with an IPython session, and you should see an initial series of statements successfully executed to establish client/server communication with TextMate.

Now you can invoke `⌃⇧I` again to run your file or selected lines in IPython, and afterward ask for a variable listing to inspect states after the execution.

### Debugging ###

From TextMate you can toggle on and off automatic entering of the `pdb` [debugger][] when exceptions occur, set and clear breakpoints, and invoke a post-mortem debugger to load the context of the last exception that occurred, in case you forgot to turn on `pdb` beforehand.

[debugger]: http://docs.python.org/library/pdb.html#debugger-commands

### Snippets ###

As usual, you can open the Bundle Editor to explore the completion snippets provided by the bundle, currently comprised of Scipy-, matplotlib- and Enthought Tool Suite-related code.

### Configuration Files ###

A simple language grammar for highlighting `ipythonrc` files in also included. You can switch the language grammar to the `ipythonrc` type by pressing `⌃⌥⇧I` (Shift + Cmd + Alt + I). A bundle command is also available for easy editing of the `ipythonrc` and `ipy_user.conf` files (provided they live under `~/.ipython`).


How It Works
------------

The bundle uses a socket-based server distributed with IPython to communicate with the interpreter. Socket support is still **work in progress**. That said, most of the bundle commands use this functionality, and work well.

A few commands such as some debugger control use AppleScript, and old AppleScript versions of other commands are still available in case you have trouble with the socket server. In this case, the AppleScript commands are available in the `AppleScript Commands` group in the Menu Structure section of the Bundle Editor -- you can add the `⌃⇧I` keyboard activation to have these appear in the menu you, or you can always invoke them from the `Select Bundle Item` dialog (⌃⌘T).


Bundle Configuration
--------------------

A few aspects of bundle functionality can be configured by setting environment variables in `Preferences > Advanced > Shell Variables` in TextMate, as follows.

### How IPython is Run ###

If you wish to specify the arguments with which `ipython` is run, or give a path to an `ipython` executable in a virtualenv, you may set `TM_IPYTHON` in TextMate's preferences. I recommend setting this variable to `ipython -pylab -wthread` for loading IPython's special support for SciPy/matplotlib.


### Socket Server and Initialization ###

The editor server creates a UNIX socket file in `~/.ipython`. By default, it is named `textmate-session` -- if you should wish to change this for some reason, set `TM_IPYTHON_DEFAULT_SESSION`.

`TM_IPYTHON_DEFAULT_SESSION` will only be interpreted as a string, so it cannot be a dynamic value and thus the bundle normally supports a single IPython session running at a time. If you would like to try interacting with multiple sessions -- perhaps you have two TextMate projects open and wish to run different IPython sessions for each of them -- you can set a custom initialization routine in `TM_IPYTHON_START_SERVER_COMMAND`.

By default, `TM_IPYTHON_START_SERVER_COMMAND` is essentially this:

    import ipy_vimserver; ipy_vimserver.setup("textmate-session")

`ipy_vimserver.py` is distributed with IPython and is made available in the load path by default. To run multiple sessions, you might try setting the variable at TM project level to something like:

    import os; import ipy_vimserver; ipy_vimserver.setup('session-%s' % os.path.basename(os.environ['TM_PROJECT_DIRECTORY']))

Alternatively, you could handle session setup this way in your `~/ipython/ipy_user_conf.py` file and then omit usage of the `Open IPython` bundle command -- the bundle will try to find sockets in `~/.ipython` when you run commands.

This bundle may include additional support for interacting with multiple sessions in the future, but for now this is largely untested.

<!-- TODO: include the Twisted-based server in the bundle and document here -->

Troubleshooting
---------------

The above information on session initialization configuration gives some hint as to how you might attempt to troubleshoot if you have problems -- probably, issues would relate to connection to the socket server, so you can try to set it up manually and look for errors. Run `ipython` manually in a terminal and enter:

    import ipy_vimserver
    ipy_vimserver.setup('socketname')

This will create a unix socket called `socketname`, in `~/.ipython/`. If there are no errors, try to run the bundle's `Test Connection to IPython Server` command, and success or failure should be reported in a window.

You could also try the AppleScript versions of some commands, as discussed above.


Installation
------------

This bundle is best installed using GetBundles. You can also clone it from GitHub, or download a tarball.

To install GetBundles:

1. Open a Terminal
2. Type: `cd; svn co http://svn.textmate.org/trunk/Review/Bundles/GetBundles.tmbundle/`
3. Type: `open GetBundles.tmbundle`

To use GetBundles:

1. Switch to TextMate
2. Start a new document
3. Choose `GetBundles` → `Get Bundles` from the `Bundles` menu.
4. Browse or search the list, select `IPython` and hit `Install Bundles`

It is also advisable to update the support directory, which can be done using the `Advanced Drawer` in GetBundles.


Acknowledgements
----------------

The bundle was started after seeing [UsingIPythonWithTextMate][], containing the AppleScripts on which the core commands of early versions of this bundle were based and which are still available as an alternative to the socket server for some features. Kudos to Barry Wark for writing and sharing these.

[UsingIPythonWithTextMate]: http://ipython.scipy.org/moin/Cookbook/UsingIPythonWithTextMate "Cookbook/UsingIPythonWithTextMate - IPython"


Help, Suggestions or Feature Requests
-------------------------------------

For help, please use the [Google Group][], for suggestions, feature requests, bug reports or patches, please post to the [IPython-dev mailing list][], please prefix the subject line with `[TextMate]`.

[Google Group]: http://groups.google.com/group/ipython-tmbundle/
[IPython-dev mailing list]: http://projects.scipy.org/mailman/listinfo/ipython-dev "IPython-dev Info Page"


Useful Links
------------

  * [IPython bundle](http://github.com/mattfoster/ipython-tmbundle) on [GitHub](http://github.com/ "Secure Git hosting and collaborative development &mdash; GitHub")
  * [IPython Home](http://ipython.scipy.org "IPython")
  * [Scipy](http://www.scipy.org/ "SciPy")

<!--
The bundle uses a socket-based server distributed with IPython to communicate with the interpreter. You will need a fairly recent version of IPython installed for the requisite editor server support to be included. You may also need to install Twisted in your Python environment -- in OS X Snow Leopard it is installed by default for the system Python.
-->

[Twisted]: http://twistedmatrix.com/trac/ "Twisted"

