# A hacky way to send notifications from a development machine on iTerm2

When hacking on something in a language that's compiled, some compiles take a long time. 
So long that it doesn't make sense to wait and watch it. You can do some lightweight tasks in the meantime - 
respond to emails, review diffs...
The problem is, unless you leave your terminal visible, you won't know it's done the moment it is done.

The solution is to send a notification once it's compiled. 
This is not a new idea, `iTerm2` has support for notifications from within a terminal via a special escape code.
Problem solved? Kind of. If you only use `iTerm2`, then it works like a charm. 
But if you use a combination of `iTerm2` + `mosh` + `tmux` the escape code gets lost somewhere and `iTerm2` never sees it.

So I came up with a hack. The idea is to use `iTerm2`'s built-in trigger system. 
`iTerm2` lets you set up a trigger, so that when a certain text appears it runs a command. Let's use it!

First we need to set up the trigger. In the `Session preferences` on the `Advanced` tab there's a `Triggers` section.
The trigger you want to add is:

Regular Expression | Action | Parameters
------------------ | ------ | ----------
`iTermNotify (.*)`   | Run Command... | `osascript -e 'display notification "\1" with title "iTerm2"'`

Then on your devserver you want to add this function to your `.bashrc`:

```
function iterm_notify() {
  echo && echo -en iTerm""Notify "$@\r" && sleep 1 && echo "     "
}
```

And then when want to you get notified after the build finishes you do:

```
$ make; iterm_notify "build finished!"
```

`iterm_notify` deserves a bit of explanation. Why not just `echo -en iTermNotify "$@"`? 
The problem is, if you open `vim` right after the notification was shown and close it, the notification will happen again.
To solve this, I first write the desired string without a newline (`-n` option for `echo`) and 
then use the carriage return character (`"\r"`) to go to the beginning of the line and overwrite the first few characters with the second `echo`.
This didn't work reliably before I added `sleep 1` in between and a newline before (first `echo`). Now for the last part.
Why `iTerm""Notify` instead of `iTermNotify`? This is because I don't want a notification to pop-up every time I edit my `.bashrc`.

Hope this is useful to someone, enjoy!
