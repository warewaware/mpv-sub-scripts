# sub-pause

An mpv script that automatically pauses after each subtitle line.
Mostly intended for language learning.

The default key bindings are:

- `n`: toggle auto-pause (`sub-pause-toggle`)
- `Ctrl+SPACE`: skip next pause (`sub-pause-skip-next`)
- `Alt+SPACE`: replay active line, always available (`sub-pause-replay`)

To use custom key bindings add lines like the following to your `input.conf`
(where `action_name` is the value in parentheses in the above list):

```X script-binding <action_name>```

If you want a binding for both replaying and skipping add a line like this:

```Ctrl+Alt+SPACE script-binding sub-pause-replay; script-binding sub-pause-skip-next```

## Known Issues

If there are multiple subtitles visible at the same time (e.g. one at
the top and one at the bottom, or on-screen text with SubStation Alpha
(ASS)) the script will only pause at the end of the last visible line.

To my knowledge, there is no simple fix for this due to the way mpv
presents subtitle timing information to user scripts.

A more involved solution would be to parse the sub file externally,
but that would probably not be worth the effort considering how rarely
the issue is likely to occur.

---

If you come across any other problems while using the script feel free
to open a GitHub issue or send a pull request.

# sub-skip

This script allows automatically skipping parts of a video that don't contain any subtitles.
Skipping can be done either by speeding up playback while no subtitles are present or by
seeking to the start of the next subtitle line, skipping the interval between lines entirely.

## How To Use

The script works by briefly changing subtitle delay so that the next line starts at the current
video/audio time, recording the difference between the original and shifted subtitle delay and
then speeding up or seeking to the start of the next line (calculated from the difference).

This works best if mpv is forced to build a cache even for local fikes by setting `cache=yes`
in the config. Alternatively, setting `demuxer-readahead-secs` to 60 or some other value that
is likely to be larger than the interval between any two subs works as well. 

If cache or muxer readahead are not set explicitly it is possible for the next subtitle line to
be unavailable even if it starts withing the next seconds. The script can deal with this but
works best if one of the config entries from above is set.

## Key Bindings

- `Ctrl+Alt+n`: toggle seek skip mode (`sub-skip-switch-mode`)
- `Ctrl+Alt+[`: decrease skip speed by 0.1 (`sub-skip-decrease-speed`)
- `Ctrl+Alt+]`: increase skip speed by 0.1 (`sub-skip-increase-speed`)
- `Ctrl+Alt+-`: decrease skip interval by 0.25s (`sub-skip-decrease-interval`)
- `Ctrl+Alt++`: increase skip interval by 0.25s (`sub-skip-increase-interval`)

All actions can be rebound as explained for `sub-pause`.
