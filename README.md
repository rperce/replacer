replacer
---

A few-frills templating engine for any sort of file you throw at it. Call it like

    ./render <template-file> [-c configfilename]

By default, it looks for a config file called "vars." That file should look like this:

    replace me with: anything on the right side of the colon

    lines with no colons are ignored, unless they are after lines ending with backslash;
    similarily lines with colons disregard their colon if they are a continuation line.

    foo: Any replacement that ends in a single backslash \
         will have the next line appended to the first. \
         No spaces will be inserted, but whitespace on a continuation line up to \
    the first non-whitespace column of the very first line of a continuation \
         will be stripped: \
         <--- that will only be one space, because the previous line had a space \
         before the backslash, whereas\
          <--- that is also a single space, but it is on the same line as the arrow.

    bar: Any replacement that ends in a double backslash \\
         will have the next line appended, with the same whitespace-trimming rule, \\
         but with a newline character (platform-dependent) in-between.

     !@ #$   : there is no such thing as a special character in the key, but it is trimmed \
               before processing, so there must be at least one non-whitespace character. \\
               Internal spaces are fine

And the template file should look something like this:

    This is a template file. `replacer` doesn't care about any characters in this file
    except something in double mustaches: { { with no space in between followed by } }. If
    it sees that, it will replace according to the config file, replacing anything that
    has an entry with {{ replace me with }} corresponding to that key.

    Note some behavior:
    {{foo}}

    {{bar}}

    And remember, {{!@ #$}}.

The output of those files, included here as `vars.demo` and `template.demo`, is:

    This is a template file. `replacer` doesn't care about any characters in this file
    except something in double mustaches: { { with no space in between followed by } }. If
    it sees that, it will replace according to the config file, replacing anything that
    has an entry with anything on the right side of the colon corresponding to that key.

    Note some behavior:
    Any replacement that ends in a single backslash will have the next line appended to the first. No spaces will be inserted, but whitespace on a continuation line up to irst non-whitespace column of the very first line of a continuation will be stripped: <--- that will only be one space, because the previous line had a space before the backslash, whereas <--- that is also a single space, but it is on the same line as the arrow.

    Any replacement that ends in a double backslash 
    will have the next line appended, with the same whitespace-trimming rule, 
    but with a newline character (platform-dependent) in-between.

    And remember, there is no such thing as a special character in the key, but it is trimmed before processing, so there must be at least one non-whitespace character. 
    Internal spaces are fine.
