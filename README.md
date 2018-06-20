# nb
Simple command line note management tool

```
USAGE:
  nb                     : list the notes
  nb ls                  : list the notes
  nb add [<NAME>]        : add a note to <NAME> (or the default)
  nb cat [<NAME>]        : cat notes in <NAME> (or the default)
  nb view [<NAME>]       : view notes in <NAME> with your pager
  nb edit [<NAME>]       : open notes <NAME> in your editor
  nb rm [<NAME>]         : delete notes <NAME>
  nb del [<NAME>]        : delete notes <NAME>
  nb mv <NAME1> <NAME2>  : rename notes from <NAME1> to <NAME2>

  nb default <NAME>      : Sets the current note as the default
                           Currently 'os'
  nb autosave [0|1]      : Sets autosave on/off (must have a git repo set up)
                           Currently '1'
  nb save                : Saves the current notes to git
  nb push                : Pushes the changes
  nb pull                : Pulls the changes
  nb completions         : Prints the completion function
  nb get_notes           : List the completions (for use programatically)

  nb -h,--help           : show this message

ENV:
  $NOTES_DIR             : The directory in which notes are stored
```

## Simple usage

Add a new note:
```bash
$ nb add foo
Adding note "foo".  Hit ctrl-d when done...
Hi, this is a test.
```

View the names of all notes:
```bash
 $ nb ls
 - foo
 - os
```

Retrieve a note:
```bash
 $ nb cat foo
Hi, this is a test.
```

Or to open it with your pager (e.g. `more` or `less`):
```bash
 $ nb view foo
Hi, this is a test.
```

Edit a note:
```bash
 $ nb edit foo
[Opens your editor]
```

Rename a note:
```bash
 $ nb mv foo bar
```

Remove a note:
```bash
 $ nb rm bar
Deleting note "bar"...
```

## Grouping notes

If your notes have `/`s in them then it will make directories to
create the nodes.  If all of the notes are removed from a directory,
then the directory will be removed.

## Advanced usage

### Default note

You can set a default note that will be used for `add`, `cat`, `view`, `edit`, `rm`, and `del`:
```bash
 $ nb default bar
Set default note to 'bar'

 $ nb cat
Hi, this is a test.
```

### Bash completions

The `nb` command can be used to set the bash completions.  If you run
`nb completions` it will print the completion function and hook to
use.  You can save that to your `bashrc`, or you can run it
dynamically to load the completions:

```bash
 $ eval "$(nb completions)"
```

### Git integration

`nb` supports saving changes with `git` and auto-pushing them to
remote repositories.  You have to set up the repository yourself:

```bash
 $ cd ~/notes
 $ git init
Initialized empty Git repository in /home/bbennett/notes/.git/
 $ git add .
 $ git ci -m "Initial commit"
[master (root-commit) 9e2db05] Initial commit
 1 file changed, 1 insertion(+)
 create mode 100644 test
```

Then you can save the state of your notes with:

```bash
 $ nb save
--
On branch master

nothing to commit, working tree clean
```

If you want to automatically save all of the notes after every change
then you can enable autosaving with:
```bash
 $ nb autosave 1
Set auto_save to '1'
--
[master d6113ad] Save at Mon Jun 18 11:34:58 EDT 2018
 1 file changed, 1 insertion(+), 1 deletion(-)
```

You'll note that it immediately saved the config file change since
auto-saving is enabled.

If you want to save to a remote repository too, you can set up remotes
in git and then push the changes with:
```bash
 $ nb push
Everything up-to-date
```

Or if you are syncing notes from multiple places, you can pull with:
```bash
 $ nb pull
Already up to date.
```

But it will not do any sophisticated merging if there are changes from
multiple places.  You may need to go to your notes dir and use git to
merge if that is the case.