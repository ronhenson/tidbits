---
title: "Keepass auto type codes"
created: 2021-06-14 06:07:23
tags: [ keepass ]
---

# [Keepass auto type codes][14]

## Basic Auto-Type Information

KeePass features an "Auto-Type" functionality. This feature allows you to define a sequence of keypresses, which KeePass can automatically perform for you. The simulated keypresses can be sent to any other currently open window of your choice (browser windows, login dialogs, ...).

By default, the sent keystroke sequence is `{USERNAME}{TAB}{PASSWORD}{ENTER}`, i.e. it first types the user name of the selected entry, then presses the Tab key, then types the password of the entry and finally presses the Enter key.

For [TAN entries][1], the default sequence is `{PASSWORD}`, i.e. it just types the TAN into the target window, without pressing Enter.

_KeePass 2.x Only_

Auto-Type can be configured individually for each entry using the *Auto-Type* tab page on the entry dialog (select an entry → *Edit Entry*). On this page you can specify a default sequence and customize specific window/sequence associations.
[Two-Channel Auto-Type Obfuscation][2] is supported (making Auto-Type resistant against keyloggers).

Additionally, you can create customized window/sequence associations, which override the default sequence. You can specify different keystroke sequences for different windows for each entry. For example, imagine a webpage, to which you want to login, that has multiple pages where one can login. These pages could all look a bit different (on one you could additionally need to check some checkbox – like often seen in forums). Here creating customized window/sequence associations solves the problems: you simply specify different auto-type sequences for each windows (identified by their window titles).

**Invoking Auto-Type:**
There are three different methods to invoke auto-type:

- Invoke auto-type for an entry by using the context menu command *Perform Auto-Type* while the entry is selected.
- Select the entry and press Ctrl+V (that's the menu shortcut for the context menu command above).
- Using the system-wide auto-type hot key. KeePass will search all entries in the currently opened database for matching sequences.

All methods are explained in detail below.

**Input Focus:**
Note that auto-type starts typing into the control of the target window that has the input focus. Thus, for example for the default sequence you have to ensure that the input focus is set to the user name control of the target window before invoking auto-type using any of the above methods.

## ![Info](https://keepass.info/help/images/b16x16_help.png) Requirements and Limitations

**Rights:**
For auto-type to work, KeePass must be running with the same or higher rights as the target application. Especially, if the target application is running with administrative rights, KeePass must be running with administrative rights, too. For details, see [Windows Integrity Mechanism Design][3]. An example are certain instances of VMware Workstation that run on a higher integrity level.

**Remote Desktops and Virtual Machines:**
KeePass does not know the keyboard layout that has been selected in a remote desktop or virtual machine window. If you want to auto-type into such a window, you must ensure that the local and the remote/virtual system are using the same keyboard layout.

When performing auto-type into a remote desktop or virtual machine window, the following characters may be problematic (depending on the exact circumstances) and should therefore be avoided, if possible: `"` (U+0022), `'` (U+0027), `^` (U+005E), `` ` `` (U+0060), `~` (U+007E), `¨` (U+00A8), `¯` (U+00AF), `°` (U+00B0), `´` (U+00B4), `¸` (U+00B8), [spacing modifier letters][4] (U+02B0 to U+02FF), and characters that cannot be realized with a direct key combination.

**Wayland:**
On a Unix-like system with a Wayland compositor, there may be further limitations; see the [Auto-Type on Wayland][5] page.

## ![Keyboard](https://keepass.info/help/images/b16x16_ktouch.png) Context Menu: '*Perform Auto-Type*' Command

This method is the one that requires the least amount of configuration and is the simpler one, but it has the disadvantage that you need to select the entry in KeePass which you want to auto-type.

The method is simple: right-click on an entry of your currently opened database and click '*Perform Auto-Type*' (or alternatively press the Ctrl+V shortcut for this command). The window that previously got the focus (i.e. the one in which you worked before switching to KeePass) will be brought to the foreground and KeePass auto-types into this window.

The sequence which is auto-typed depends on the window's title. If you didn't specify any custom window/sequence associations, the default sequence is sent. If you created associations, KeePass uses the sequence of the first matching association. If none of the associations match, the default sequence is used.

## ![Keyboard](https://keepass.info/help/images/b16x16_ktouch.png) Global Auto-Type Hot Key

This is the more powerful method, but it also requires a little bit more work/knowledge, before it can be used.

**Simple Global Auto-Type Example:**

1. Create an entry in KeePass titled *Notepad* with values for user name and password.
2. Start Notepad (under 'Programs' → 'Accessories').
3. Press Ctrl+Alt+A within Notepad. The user name and password will be typed into Notepad.

The KeePass entry title *Notepad* is matched with the window title of Notepad and the default Auto-Type sequence is typed.

**How It Works - Details:**

KeePass registers a system-wide hot key for auto-type. The advantage of this hot key is that you don't need to switch to the KeePass window and select the entry. You simply press the hot key while having the target window open (i.e. the window which will receive the simulated keypresses).

By default, the global hot key is Ctrl+Alt+A (i.e. hold the Ctrl and Alt keys, press A and release all keys). You can change this hot key in the options dialog (main menu - 'Tools' - 'Options', tab 'Integration'/'Advanced'): here, click into the textbox below "Global Auto-Type Hot Key Combination" and press the hot key that you wish to use. If the hot key is usable, it will appear in the textbox.

When you press the hot key, KeePass looks at the title of the currently opened window and searches the currently opened database for usable entries. If KeePass finds multiple entries that can be used, it displays a selection dialog. An entry is considered to be usable for the current window title when at least one of the following conditions is fulfilled:

- The title of the entry is a substring of the currently active window title.
- The entry has a window/sequence association, of which the window specifier matches the currently active window title.

The second condition has been mentioned already, but the first one is new. By using entry titles as filters for window titles, the configuration amount for auto-type is almost zero: you only need to make sure that the entry title is contained in the window title of the window into which you want the entry to be auto-typed. Of course, this is not always possible (for example, if a webpage has a very generic title like *"Welcome"*), here you need to use custom window/sequence associations.

_KeePass 2.x Only_

Custom window/sequence associations can be specified on the *'Auto-Type'* tab page of each entry.
The associations complement the KeePass entry title. Any associations specified will be used in addition to the KeePass entry title to determine a match.

Auto-Type window definitions, entry titles and URLs are Spr-compiled, i.e. [placeholders][6], [environment variables][7], [field references][8], etc. can be used.

## ![Keyboard](https://keepass.info/help/images/b16x16_ktouch.png) Auto-Type Keystroke Sequences

An auto-type keystroke sequence is a one-line string that can contain placeholders and special key codes.

A complete list of all supported placeholders can be found on the page [Placeholders][6]. The special key codes can be found below.

Above you've seen already that the default auto-type is `{USERNAME}{TAB}{PASSWORD}{ENTER}`. Here, `{USERNAME}` and `{PASSWORD}` are placeholders: when auto-type is performed, these are replaced by the appropriate field values of the entry. `{TAB}` and `{ENTER}` are special key codes: these are replaced by the appropriate keypresses. Special key codes are the only way to specify special keys like Arrow-Down, Shift, Escape, etc.

Of course, keystroke sequences can also contain simple characters to be sent. For example, the following string is perfectly valid as keystroke sequence string:
`{USERNAME}{TAB}Some text to be sent!{ENTER}`.

_KeePass 2.x Only_

Special key codes are case-insensitive.

**Special Keys:**
The following codes for special keys are supported:

| Special Key | Code |
| --- | --- |
| Tab | `{TAB}` |
| Enter | `{ENTER}` or `~` |
| Arrow Up | `{UP}` |
| Arrow Down | `{DOWN}` |
| Arrow Left | `{LEFT}` |
| Arrow Right | `{RIGHT}` |
| Insert | `{INSERT}` or `{INS}` |
| Delete | `{DELETE}` or `{DEL}` |
| Home | `{HOME}` |
| End | `{END}` |
| Page Up | `{PGUP}` |
| Page Down | `{PGDN}` |
| Space | `{SPACE}` |
| Backspace | `{BACKSPACE}`, `{BS}` or `{BKSP}` |
| Break | `{BREAK}` |
| Caps-Lock | `{CAPSLOCK}` |
| Escape | `{ESC}` |
| Windows Key | `{WIN}` (equ. to `{LWIN}`) |
| Windows Key: left, right | `{LWIN}`, `{RWIN}` |
| Apps / Menu | `{APPS}` |
| Help | `{HELP}` |
| Numlock | `{NUMLOCK}` |
| Print Screen | `{PRTSC}` |
| Scroll Lock | `{SCROLLLOCK}` |
| F1 - F16 | `{F1}` \- `{F16}` |
| Numeric Keypad + | `{ADD}` |
| Numeric Keypad - | `{SUBTRACT}` |
| Numeric Keypad * | `{MULTIPLY}` |
| Numeric Keypad / | `{DIVIDE}` |
| Numeric Keypad 0 to 9 | `{NUMPAD0}` to `{NUMPAD9}` |
| Shift | `+` |
| Ctrl | `^` |
| Alt | `%` |

_KeePass 2.x Only_

|     |     |
| --- | --- |
| Special Key | Code |
| +   | `{+}` |
| %   | `{%}` |
| ^   | `{^}` |
| ~   | `{~}` |
| (, ) | `{(}`, `{)}` |
| \[, \] | `{[}`, `{]}` |
| {, } | `{{}`, `{}}` |

Additionally, some special commands are supported:

|     |     |
| --- | --- |
| Command Syntax | Action |
| `{DELAY X}` | Delays *X* milliseconds. |
| `{DELAY=X}` | Sets the default delay to *X* milliseconds for all following keypresses. |
| `{CLEARFIELD}` | Clears the contents of the edit control that currently has the focus (only single-line edit controls). |
| `{VKEY X}` | Sends the [virtual key][9] of value *X*. |
| `{APPACTIVATE WindowTitle}` | Activates the window "*WindowTitle*". |
| `{BEEP X Y}` | Beeps with a frequency of *X* hertz and a duration of *Y* milliseconds. |

_KeePass 2.x Only_

|     |     |
| --- | --- |
| Command Syntax | Action |
| `{VKEY X F}` | Sends the [virtual key][9] of value *X*; see [below][10]. |

_KeePass 2.x Only_

**`{VKEY X F}`:**
This command sends the [virtual key][9] of value *X*. The parameter *F* is optional and may be a combination of the following values:

- **`E`**: Send an [extended key][11]; see below.
- **`N`**: Send a non-extended key; see below.
- **`D`**: Press and hold down the key (without releasing it).
- **`U`**: Release the key (without pressing it).

The values `E` and `N` are mutually exclusive. It is recommended to specify neither `E` nor `N`, if possible; KeePass then determines automatically whether the virtual key is typically realized using an extended key.

The values `D` and `U` are mutually exclusive. If neither `D` nor `U` is specified, KeePass sends a keypress (i.e. down and up).

On Linux systems, KeePass automatically converts most Windows virtual key codes to Linux key codes (i.e. the `{VKEY ...}` command works on both systems).

**Examples:**

- `{VKEY 13}`
    Presses and releases the primary Enter key. This is equivalent to `{ENTER}`.
- `{VKEY 13 E}`
    Presses and releases the Enter key of the numeric keypad.
- `{VKEY 91 D}e{VKEY 91 U}`
    Sends Win+E (i.e. it presses and holds down the left Win key, presses and releases the E key, and releases the Win key), which starts Windows Explorer (on Windows). This is not equivalent to `{LWIN}e` (which first presses and releases the left Win key and then presses and releases the E key).
    Note that Windows Explorer can also be started using `{CMD:/Explorer.exe/W=0/}` (the [`{CMD:/.../}`][12] placeholder can run arbitrary command lines).

Do not use the `{VKEY ...}` command to change the state of the Shift, Ctrl and Alt modifiers. For this, use `+`, `^` and `%` instead (see above).

_KeePass 2.x Only_

Keys and special keys (not placeholders or commands) can be repeated by appending a number within the code. For example, `{TAB 5}` presses the Tab key 5 times.

**Examples:**

`{TITLE}{TAB}{USERNAME}{TAB}{PASSWORD}{ENTER}`
Types the entry's title, a Tab, the user name, a Tab, the password of the currently selected entry, and presses Enter.

`{TAB}{PASSWORD}{ENTER}`
Presses the Tab key, enters the entry's password and presses Enter.

`{USERNAME}{TAB}^v{ENTER}`
Types the user name, presses Tab, presses Ctrl+V (which pastes data from the Windows clipboard in most applications), and presses Enter.

**Toggling Checkboxes:**
A checkbox (e.g. "Stay logged in on this computer") can usually be toggled by sending a space character (`' '`). Example:
`{USERNAME}{TAB}{PASSWORD}{TAB} {TAB}{ENTER}`
If there is a form with a user name field, a password field and a checkbox, this sequence would enter the user name, the password and toggle the checkbox that follows the password control.

**Pressing Non-Default Buttons:**
Pressing non-default buttons works the same as toggling checkboxes: send a space character (`' '`). Note that this should only be used for non-default buttons; for default buttons, `{ENTER}` should be sent instead.

**Higher ANSI Characters:**
The auto-type function supports sending of higher ANSI characters in range 126-255. This means that you can send special characters like ©, @, etc. without any problems; you can write them directly into the keystroke sequence definition.

## ![Windows](https://keepass.info/help/images/b16x16_window_list.png) Target Window Filters

When creating a custom window/sequence association, you need to tell KeePass how the matching window titles look like. Here, KeePass supports simple wildcards:

|     |     |
| --- | --- |
| String with Wildcard | Meaning |
| STRING | Matches all window titles that are named exactly "STRING". |
| STRING* | Matches all window titles that start with "STRING". |
| *STRING | Matches all window titles that end with "STRING". |
| \*STRING\* | Matches all window titles that have "STRING" somewhere in the window title. This includes |

_KeePass 2.x Only_

Wildcards may also appear inKeePass 2.x Onl the middle of patterns. For example, `*Windows*Explorer*` would match `Windows Internet Explorer`.
Additionally, matching using [regular expressions][13] is supported. In order to tell KeePass that the pattern is a regular expression, enclose it in `//`. For example, `//B.?g Window//` would match `Big Window`, `Bug Window` and `Bg Window`.

By using wildcards, you can make your auto-type associations browser-independent. See the usage examples for more information.

## ![Keyboard](https://keepass.info/help/images/b16x16_ktouch.png) Change Default Auto-Type Sequence

The default auto-type sequence (i.e. the one which is used when you don't specify a custom one) is `{USERNAME}{TAB}{PASSWORD}{ENTER}`. KeePass allows you to change this default sequence. Normally you won't need to change it (use custom window/sequence definitions instead!), but it is quite useful when some other application is interfering with KeePass (for example a security software that always asks you for permission before allowing KeePass to auto-type).

__KeePass 2.x Only__

By default, entries inherit the auto-type sequence of their containing group. Groups also inherit the auto-type sequence of their parent groups. There is only one top group (the first group contains all other groups). Consequently, if you change the auto-type sequence of this very first group, all other groups and their entries will use this sequence. Practically, this is a global override. To change it, right-click on the first group, choose *'Edit Group'* and switch to the *'Auto-Type'* tab.

## ![Text](https://keepass.info/help/images/b16x16_ascii.png) Usage Example

Now let's have a look at a real-world example: logging into a website. In this example, will we use the global auto-type hot key to fill out the login webpage. First open the [test page][15], and afterwards create a new entry in KeePass with title *Test Form* and a user name and password of your choice.

Let's assume the global auto-type hot key is set to Ctrl+Alt+A (the default). KeePass is running in the background, you have opened your database and the workspace is unlocked.

When you now navigate to the test page and are being prompted for your user name and password, just click into the user name field and press Ctrl+Alt+A. KeePass enters the user name and password for you!

Why did this work? The window title of your browser window was *"Test Form - KeePass - Internet Explorer"* or *"Test Form - KeePass - Mozilla Firefox"*, depending on the browser you are using. Because we gave the entry in KeePass the title *Test Form*, the entry title is contained in the window title, therefore KeePass uses this entry.

Here you see the huge advantages of auto-type: it not only doesn't require any additional browser software (the browser knows nothing of KeePass – there are no helper browser plugins required), it is also browser-independent: the one entry that you created within KeePass works for Internet Explorer *and* Mozilla Firefox (and other browsers) without requiring any modifications or definitions.

When you would use window/sequence associations (instead of entry title matching), you can achieve the same browser-independent effect using wildcards: you could for example have used `Test Form - KeePass - *` as window filter. This filter matches both the Internet Explorer and the Firefox window.

[1]: https://keepass.info/help/base/tans.html "TAN entries"
[2]: https://keepass.info/help/v2/autotype_obfuscation.html "Two-Channel Auto-Type Obfuscation"
[3]: https://docs.microsoft.com/en-us/previous-versions/dotnet/articles/bb625963%28v=msdn.10%29 "Windows Integrity Mechanism Design"
[4]: https://en.wikipedia.org/wiki/Spacing_Modifier_Letters "spacing modifier letters"
[5]: https://keepass.info/help/kb/autotype_wayland.html "Auto-Type on Wayland"
[6]: https://keepass.info/help/base/placeholders.html "placeholders"
[7]: https://keepass.info/help/base/placeholders.html#envvars "environment variables"
[8]: https://keepass.info/help/base/fieldrefs.html "field references"
[9]: https://docs.microsoft.com/en-us/windows/win32/inputdev/virtual-key-codes "virtual key"
[10]: https://keepass.info/help/base/autotype.html#vkey "below"
[11]: https://docs.microsoft.com/en-us/windows/win32/inputdev/about-keyboard-input#extended-key-flag "extended key"
[12]: https://keepass.info/help/base/placeholders.html#cmd "`{CMD:/.../}`"
[13]: https://docs.microsoft.com/en-us/dotnet/standard/base-types/regular-expression-language-quick-reference "regular expressions"
[14]: https://keepass.info/help/base/autotype.html "Keepass Auto-Type"
[15]: https://keepass.info/help/kb/testform.html "test page"
