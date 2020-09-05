Notesplit
=========

You use ``notesplit``, when you want to split and combine plain text
notes files for sharing with different parties, simply denoteding
inside the plain-text notes, what parts of text to share and with whom,
like ``{:whom| .. my shared text .. :}``, where ``whom`` is either
individual or group, and exclude whom not to share to by
``{:whom|{:-notwhom| .. shared text .. :}``, and where you can share
already shared parts with multiple parties, like
``{:whom1| hello {:whom2|world:} :}``, which is sufficient to cover
many cases.

It is useful in combination with file sharing software like `syncthing <https://syncthing.net/>`__ to synchronize ideas and data with specific friends.

.. code:: bash

    $ notesplit --help

    usage: notesplit [-h] [-s SOURCE] [-g GROUPS]

    optional arguments:
      -h, --help            show this help message and exit
      -s SOURCE, --source SOURCE
                            Source text file to parse.
      -g GROUPS, --groups GROUPS
                            Groups definitions json file.


Usage
=====

Running ``python split.py -s OnePage.wiki -g GroupDefinitions.wiki``
will split a source file ``OnePage.wiki`` and copy it to the folders
defined in ``GroupDefinitions.wiki``, implementing sharing with one
friend, sharing with group of friends, or sharing with group and
excluding parts of the content from a particular friend, or another
group (group intersections).

User story
----------

Imagine that you write your private diary in a text file, and want to
share a part of it with someone else's diary.

**OnePage.wiki**

.. code:: wiki

    This is your private wiki...

    By default, the diary is your private diary...
    Unless, you want {:all|SOMETHING:} all of your friends
    to see, or one of your friends to see {:friend1| JUST FOR YOU :},
    or a group of friends to see, say {:group1| MY DEAR ONES :}.

    Or, you sometimes want to share with a group, but exclude someone, or some subgroup:

    {:group1|
    == Example Story ==
    One day, I realized that we could use shared diaries on VIM, and I hacked a solution to let my dear friend also see my diary. We started writing diaries together, side-by-side, every day. We share them via Dropbox, but encrypted, and using gnupg plugin for VimWiki.

    It is a wonder to share minds like that together. I think it is like being two hemispheres of brain, connected via corpus callosum. We merged to form something new! Two minds working in unison.

    {:-group2|Then. We thought we should share more with our friends, and we found BTSync, which is like Dropbox, but P2P. It was the solution, because we didn't need to teach every friend how to use GPG and VIM. However, there is a little problem that we would like to fix, but have no time right now.:}

    We already have a Python script {:-friend1|( https://github.com/Mindey/diary-scripts/blob/master/diary-cron.py ) :}that does something similar. We would like to have a general solution, which goes as deep into the hierarchy defined by nested braces {: :} as needed to parse them.
    :}

This is your groups.. **GroupDefinitions.wiki**

.. code:: json

    {
        "individuals": {
            "friend1": "./wiki/friend1",
            "friend2": "./wiki/friend2",
            "friend3": "./wiki/friend3"
        },
        "groups": {
            "all": ["friend1", "friend2", "friend3"],
            "group1": ["friend1","friend2"],
            "group2": ["friend2"]
        }
    }

You get the splits made into the folders defined, and then, you can use
something like `syncthing <https://syncthing.net/>`__ to synchronize
each of the folders with specific friends.