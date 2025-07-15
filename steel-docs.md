# /Users/mkurzynski/.local/share/steel/cogs/helix/ext.scm
### **eval-buffer**
Eval the current buffer, morally equivalent to load-buffer!
### **evalp**
Eval prompt
### **running-on-main-thread?**
Check what the main thread id is, compare to the main thread
### **hx.with-context**
If running on the main thread already, just do nothing.
Check the ID of the engine, and if we're already on the
main thread, just continue as is - i.e. just block. This does
not block on the function if this is running on another thread.

```scheme
(hx.with-context thunk)
```
thunk : (-> any?) ;; Function that has no arguments

# Examples
```scheme
(spawn-native-thread
  (lambda () 
    (hx.with-context (lambda () (theme "nord")))))
```
### **hx.block-on-task**
Block on the given function.
```scheme
(hx.block-on-task thunk)
```
thunk : (-> any?) ;; Function that has no arguments

# Examples
```scheme
(define thread
  (spawn-native-thread
    (lambda () 
      (hx.block-on-task (lambda () (theme "nord") 10)))))

;; Some time later, in a different context - if done at the same time,
;; this will deadline, since the join depends on the callback previously
;; executing.
(equal? (thread-join! thread) 10) ;; => #true
```
# /Users/mkurzynski/.local/share/steel/cogs/helix/commands.scm
### **quit**
Close the current view.
### **quit!**
Force close the current view, ignoring unsaved changes.
### **open**
Open a file from disk into the current view.
### **buffer-close**
Close the current buffer.
### **buffer-close!**
Close the current buffer forcefully, ignoring unsaved changes.
### **buffer-close-others**
Close all buffers but the currently focused one.
### **buffer-close-others!**
Force close all buffers but the currently focused one.
### **buffer-close-all**
Close all buffers without quitting.
### **buffer-close-all!**
Force close all buffers ignoring unsaved changes without quitting.
### **buffer-next**
Goto next buffer.
### **buffer-previous**
Goto previous buffer.
### **write**
Write changes to disk. Accepts an optional path (:write some/path.txt)
### **write!**
Force write changes to disk creating necessary subdirectories. Accepts an optional path (:write! some/path.txt)
### **write-buffer-close**
Write changes to disk and closes the buffer. Accepts an optional path (:write-buffer-close some/path.txt)
### **write-buffer-close!**
Force write changes to disk creating necessary subdirectories and closes the buffer. Accepts an optional path (:write-buffer-close! some/path.txt)
### **new**
Create a new scratch buffer.
### **format**
Format the file using an external formatter or language server.
### **indent-style**
Set the indentation style for editing. ('t' for tabs or 1-16 for number of spaces.)
### **line-ending**
Set the document's default line ending. Options: crlf, lf.
### **earlier**
Jump back to an earlier point in edit history. Accepts a number of steps or a time span.
### **later**
Jump to a later point in edit history. Accepts a number of steps or a time span.
### **write-quit**
Write changes to disk and close the current view. Accepts an optional path (:wq some/path.txt)
### **write-quit!**
Write changes to disk and close the current view forcefully. Accepts an optional path (:wq! some/path.txt)
### **write-all**
Write changes from all buffers to disk.
### **write-all!**
Forcefully write changes from all buffers to disk creating necessary subdirectories.
### **write-quit-all**
Write changes from all buffers to disk and close all views.
### **write-quit-all!**
Write changes from all buffers to disk and close all views forcefully (ignoring unsaved changes).
### **quit-all**
Close all views.
### **quit-all!**
Force close all views ignoring unsaved changes.
### **cquit**
Quit with exit code (default 1). Accepts an optional integer exit code (:cq 2).
### **cquit!**
Force quit with exit code (default 1) ignoring unsaved changes. Accepts an optional integer exit code (:cq! 2).
### **theme**
Change the editor theme (show current theme if no name specified).
### **yank-join**
Yank joined selections. A separator can be provided as first argument. Default value is newline.
### **clipboard-yank**
Yank main selection into system clipboard.
### **clipboard-yank-join**
Yank joined selections into system clipboard. A separator can be provided as first argument. Default value is newline.
### **primary-clipboard-yank**
Yank main selection into system primary clipboard.
### **primary-clipboard-yank-join**
Yank joined selections into system primary clipboard. A separator can be provided as first argument. Default value is newline.
### **clipboard-paste-after**
Paste system clipboard after selections.
### **clipboard-paste-before**
Paste system clipboard before selections.
### **clipboard-paste-replace**
Replace selections with content of system clipboard.
### **primary-clipboard-paste-after**
Paste primary clipboard after selections.
### **primary-clipboard-paste-before**
Paste primary clipboard before selections.
### **primary-clipboard-paste-replace**
Replace selections with content of system primary clipboard.
### **show-clipboard-provider**
Show clipboard provider name in status bar.
### **change-current-directory**
Change the current working directory.
### **show-directory**
Show the current working directory.
### **encoding**
Set encoding. Based on `https://encoding.spec.whatwg.org`.
### **character-info**
Get info about the character under the primary cursor.
### **reload**
Discard changes and reload from the source file.
### **reload-all**
Discard changes and reload all documents from the source files.
### **update**
Write changes only if the file has been modified.
### **lsp-workspace-command**
Open workspace command picker
### **lsp-restart**
Restarts the given language servers, or all language servers that are used by the current file if no arguments are supplied
### **lsp-stop**
Stops the given language servers, or all language servers that are used by the current file if no arguments are supplied
### **tree-sitter-scopes**
Display tree sitter scopes, primarily for theming and development.
### **tree-sitter-highlight-name**
Display name of tree-sitter highlight scope under the cursor.
### **debug-start**
Start a debug session from a given template with given parameters.
### **debug-remote**
Connect to a debug adapter by TCP address and start a debugging session from a given template with given parameters.
### **debug-eval**
Evaluate expression in current debug context.
### **vsplit**
Open the file in a vertical split.
### **vsplit-new**
Open a scratch buffer in a vertical split.
### **hsplit**
Open the file in a horizontal split.
### **hsplit-new**
Open a scratch buffer in a horizontal split.
### **tutor**
Open the tutorial.
### **goto**
Goto line number.
### **set-language**
Set the language of current buffer (show current language if no value specified).
### **set-option**
Set a config option at runtime.
For example to disable smart case search, use `:set search.smart-case false`.
### **toggle-option**
Toggle a config option at runtime.
For example to toggle smart case search, use `:toggle search.smart-case`.
### **get-option**
Get the current value of a config option.
### **sort**
Sort ranges in selection.
### **reflow**
Hard-wrap the current selection of lines to a given width.
### **tree-sitter-subtree**
Display the smallest tree-sitter subtree that spans the primary selection, primarily for debugging queries.
### **config-reload**
Refresh user config.
### **config-open**
Open the user config.toml file.
### **config-open-workspace**
Open the workspace config.toml file.
### **log-open**
Open the helix log file.
### **insert-output**
Run shell command, inserting output before each selection.
### **append-output**
Run shell command, appending output after each selection.
### **pipe**
Pipe each selection to the shell command.
### **pipe-to**
Pipe each selection to the shell command, ignoring output.
### **run-shell-command**
Run a shell command
### **reset-diff-change**
Reset the diff change at the cursor position.
### **clear-register**
Clear given register. If no argument is provided, clear all registers.
### **redraw**
Clear and re-render the whole UI
### **move**
Move the current buffer and its corresponding file to a different path
### **yank-diagnostic**
Yank diagnostic(s) under primary cursor to register, or clipboard by default
### **read**
Load a file into buffer
### **echo**
Prints the given arguments to the statusline.
### **noop**
Does nothing.
# /Users/mkurzynski/.local/share/steel/cogs/helix/components.scm
### **theme->bg**
Gets the `Style` associated with the bg for the current theme
### **theme->fg**
Gets the `style` associated with the fg for the current theme
### **theme-scope**
Get the `Style` associated with the given scope from the current theme
### **Position?**
Check if the given value is a `Position`

```scheme
(Position? value) -> bool?
```

value : any?

       
### **Style?**
Check if the given valuie is `Style`

```scheme
(Style? value) -> bool?
```

value : any?
### **Buffer?**

Checks if the given value is a `Buffer`

```scheme
(Buffer? value) -> bool?
```

value : any?
       
### **buffer-area**

Get the `Rect` associated with the given `Buffer`

```scheme
(buffer-area buffer)
```

* buffer : Buffer?
       
### **frame-set-string!**

Set the string at the given `x` and `y` positions for the given `Buffer`, with a provided `Style`.

```scheme
(frame-set-string! buffer x y string style)
```

buffer : Buffer?,
x : int?,
y : int?,
string: string?,
style: Style?,
       
### **SteelEventResult?**

Check whether the given value is a `SteelEventResult`.

```scheme
(SteelEventResult? value) -> bool?
```

value : any?

       
### **new-component!**

Construct a new dynamic component. This is used for creating widgets or floating windows
that exist outside of the buffer. This just constructs the component, it does not push the component
on to the component stack. For that, you'll use `push-component!`.

```scheme
(new-component! name state render function-map)
```

name : string? - This is the name of the comoponent itself.
state : any? - Typically this is a struct that holds the state of the component.
render : (-> state? Rect? Buffer?)
   This is a function that will get called with each frame. The first argument is the state object provided,
   and the second is the `Rect?` to render against, ultimately against the `Buffer?`.

function-map : (hashof string? function?)
   This is a hashmap of strings -> function that contains a few important functions:

   "handle_event" : (-> state? Event?) -> SteelEventResult?

       This is called on every event with an event object. There are multiple options you can use
       when returning from this function:

       * event-result/consume
       * event-result/consume-without-rerender
       * event-result/ignore
       * event-result/close

       See the associated docs for those to understand the implications for each.
       
   "cursor" : (-> state? Rect?) -> Position?

       This tells helix where to put the cursor.
   
   "required_size": (-> state? (pair? int?)) -> (pair? int?)

       Seldom used: TODO
   
### **position**

Construct a new `Position`.

```scheme
(position row col) -> Position?
```

row : int?
col : int?
       
### **position-row**

Get the row associated with the given `Position`.

```scheme
(position-row pos) -> int?
```

pos : `Position?`
       
### **position-col**

Get the col associated with the given `Position`.

```scheme
(position-col pos) -> int?
```

pos : `Position?`
### **set-position-row!**
Set the row for the given `Position`

```scheme
(set-position-row! pos row)
```

pos : Position?
row : int?
       
### **set-position-col!**
Set the col for the given `Position`

```scheme
(set-position-col! pos col)
```

pos : Position?
col : int?
       
### **Rect?**
Check if the given value is a `Rect`

```scheme
(Rect? value) -> bool?
```

value : any?

       
### **area**

Constructs a new `Rect`.

(area x y width height)

* x : int?
* y : int?
* width: int?
* height: int?

# Examples

```scheme
(area 0 0 100 200)
```
### **area-x**
Get the `x` value of the given `Rect`

```scheme
(area-x area) -> int?
```

area : Rect?
       
### **area-y**
Get the `y` value of the given `Rect`

```scheme
(area-y area) -> int?
```

area : Rect?
       
### **area-width**
Get the `width` value of the given `Rect`

```scheme
(area-width area) -> int?
```

area : Rect?
       
### **area-height**
Get the `height` value of the given `Rect`

```scheme
(area-height area) -> int?
```

area : Rect?
       
### **Widget/list?**
Check whether the given value is a list widget.

```scheme
(Widget/list? value) -> bool?
```

value : any?
       
### **widget/list**
Creates a new `List` widget with the given items.

```scheme
(widget/list lst) -> Widget?
```

* lst : (listof string?)
       
### **widget/list/render**


Render the given `Widget/list` onto the provided `Rect` within the given `Buffer`.

```scheme
(widget/list/render buf area lst)
```

* buf : `Buffer?`
* area : `Rect?`
* lst : `Widget/list?`
       
### **block**
Creates a block with the following styling:

```scheme
(block)
```

* borders - all
* border-style - default style + white fg
* border-type - rounded
* style - default + black bg
       
### **make-block**

Create a `Block` with the provided styling, borders, and border type.


```scheme
(make-block style border-style borders border_type)
```

* style : Style?
* border-style : Style?
* borders : string?
* border-type: String?

Valid border-types include:
* "plain"
* "rounded"
* "double"
* "thick"

Valid borders include:
* "top"
* "left"
* "right"
* "bottom"
* "all"
       
### **block/render**

Render the given `Block` over the given `Rect` onto the provided `Buffer`.

```scheme
(block/render buf area block)
```

buf : Buffer?
area: Rect?
block: Block?
           
       
### **buffer/clear**
Clear a `Rect` in the `Buffer`

```scheme
(buffer/clear area)
```

area : Rect?
       
### **buffer/clear-with**
Clear a `Rect` in the `Buffer` with a default `Style`

```scheme
(buffer/clear-with area style)
```

area : Rect?
style : Style?
       
### **set-color-rgb!**

Mutate the r/g/b of a color in place, to avoid allocation.

```scheme
(set-color-rgb! color r g b)
```

color : Color?
r : int?
g : int?
b : int?
### **set-color-indexed!**

Mutate this color to be an indexed color.

```scheme
(set-color-indexed! color index)
```

color : Color?
index: int?
   
### **Color?**
Check if the given value is a `Color`.

```scheme
(Color? value) -> bool?
```

value : any?

       
### **Color/Reset**

Singleton for the reset color.
       
### **Color/Black**

Singleton for the color black.
       
### **Color/Red**

Singleton for the color red.
       
### **Color/White**

Singleton for the color white.
       
### **Color/Green**

Singleton for the color green.
       
### **Color/Yellow**

Singleton for the color yellow.
       
### **Color/Blue**

Singleton for the color blue.
       
### **Color/Magenta**

Singleton for the color magenta.
       
### **Color/Cyan**

Singleton for the color cyan.
       
### **Color/Gray**

Singleton for the color gray.
       
### **Color/LightRed**

Singleton for the color light read.
       
### **Color/LightGreen**

Singleton for the color light green.
       
### **Color/LightYellow**

Singleton for the color light yellow.
       
### **Color/LightBlue**

Singleton for the color light blue.
       
### **Color/LightMagenta**

Singleton for the color light magenta.
       
### **Color/LightCyan**

Singleton for the color light cyan.
       
### **Color/LightGray**

Singleton for the color light gray.
       
### **Color/rgb**

Construct a new color via rgb.

```scheme
(Color/rgb r g b) -> Color?
```

r : int?
g : int?
b : int?
       
### **Color-red**

Get the red component of the `Color?`.

```scheme
(Color-red color) -> int?
```

color * Color?
       
### **Color-green**

Get the green component of the `Color?`.

```scheme
(Color-green color) -> int?
```

color * Color?
### **Color-blue**

Get the blue component of the `Color?`.

```scheme
(Color-blue color) -> int?
```

color * Color?
### **Color/Indexed**


Construct a new indexed color.

```scheme
(Color/Indexed index) -> Color?
```

* index : int?
       
### **set-style-fg!**


Mutates the given `Style` to have the fg with the provided color.

```scheme
(set-style-fg! style color)
```

style : `Style?`
color : `Color?`
       
### **style-fg**


Constructs a new `Style` with the provided `Color` for the fg.

```scheme
(style-fg style color) -> Style
```

style : Style?
color: Color?
       
### **style-bg**


Constructs a new `Style` with the provided `Color` for the bg.

```scheme
(style-bg style color) -> Style
```

style : Style?
color: Color?
       
### **style-with-italics**


Constructs a new `Style` with italcs.

```scheme
(style-with-italics style) -> Style
```

style : Style?
       
### **style-with-bold**


Constructs a new `Style` with bold styling.

```scheme
(style-with-bold style) -> Style
```

style : Style?
       
### **style-with-dim**


Constructs a new `Style` with dim styling.

```scheme
(style-with-dim style) -> Style
```

style : Style?
       
### **style-with-slow-blink**


Constructs a new `Style` with slow blink.

```scheme
(style-with-slow-blink style) -> Style
```

style : Style?
       
### **style-with-rapid-blink**


Constructs a new `Style` with rapid blink.

```scheme
(style-with-rapid-blink style) -> Style
```

style : Style?
       
### **style-with-reversed**


Constructs a new `Style` with revered styling.

```scheme
(style-with-reversed style) -> Style
```

style : Style?
       
### **style-with-hidden**

Constructs a new `Style` with hidden styling.

```scheme
(style-with-hidden style) -> Style
```

style : Style?
       
### **style-with-crossed-out**


Constructs a new `Style` with crossed out styling.

```scheme
(style-with-crossed-out style) -> Style
```

style : Style?
       
### **style->fg**


Return the color on the style, or #false if not present.

```scheme
(style->fg style) -> (or Color? #false)
```

style : Style?
           
       
### **style->bg**


Return the color on the style, or #false if not present.

```scheme
(style->bg style) -> (or Color? #false)
```

style : Style?
           
       
### **set-style-bg!**


Mutate the background style on the given style to a given color.

```scheme
(set-style-bg! style color)
```

style : Style?
color : Color?
           
       
### **style-underline-color**


Return a new style with the provided underline color.

```scheme
(style-underline-color style color) -> Style?

```
style : Style?
color : Color?
           
       
### **style-underline-style**

Return a new style with the provided underline style.

```scheme
(style-underline-style style underline-style) -> Style?

```

style : Style?
underline-style : UnderlineStyle?

### **UnderlineStyle?**

Check if the provided value is an `UnderlineStyle`.

```scheme
(UnderlineStyle? value) -> bool?

```
value : any?
### **Underline/Reset**

Singleton for resetting the underling style.
       
### **Underline/Line**

Singleton for the line underline style.
       
### **Underline/Curl**

Singleton for the curl underline style.
       
### **Underline/Dotted**

Singleton for the dotted underline style.
       
### **Underline/Dashed**

Singleton for the dashed underline style.
       
### **Underline/DoubleLine**

Singleton for the double line underline style.
       
### **event-result/consume**

Singleton for consuming an event. If this is returned from an event handler, the event
will not continue to be propagated down the component stack. This also will trigger a
re-render.
       
### **event-result/consume-without-rerender**

Singleton for consuming an event. If this is returned from an event handler, the event
will not continue to be propagated down the component stack. This will _not_ trigger
a re-render.
       
### **event-result/ignore**

Singleton for ignoring an event. If this is returned from an event handler, the event
will not continue to be propagated down the component stack. This will _not_ trigger
a re-render.
       
### **event-result/ignore-and-close**

Singleton for ignoring an event. If this is returned from an event handler, the event
will continue to be propagated down the component stack, and the component will be
popped off of the stack and removed.
       
### **event-result/close**

Singleton for consuming an event. If this is returned from an event handler, the event
will not continue to be propagated down the component stack, and the component will
be popped off of the stack and removed.
       
### **style**

Constructs a new default style.

```scheme
(style) -> Style?
```
       
### **Event?**
Check if this value is an `Event`

```scheme
(Event? value) -> bool?
```
value : any?
       
### **paste-event?**
Checks if the given event is a paste event.

```scheme
(paste-event? event) -> bool?
```

* event : Event?
           
       
### **paste-event-string**
Get the string from the paste event, if it is a paste event.

```scheme
(paste-event-string event) -> (or string? #false)
```

* event : Event?

       
### **key-event?**
Checks if the given event is a key event.

```scheme
(key-event? event) -> bool?
```

* event : Event?
       
### **key-event-char**
Get the character off of the event, if there is one.

```scheme
(key-event-char event) -> (or char? #false)
```
event : Event?
       
### **key-event-modifier**

Get the key event modifier off of the event, if there is one.

```scheme
(key-event-modifier event) -> (or int? #false)
```
event : Event?
       
### **key-modifier-ctrl**

The key modifier bits associated with the ctrl key modifier.
       
### **key-modifier-shift**

The key modifier bits associated with the shift key modifier.
       
### **key-modifier-alt**

The key modifier bits associated with the alt key modifier.
       
### **key-modifier-super**

The key modifier bits associated with the super key modifier.
       
### **key-event-F?**
Check if this key event is associated with an `F<x>` key, e.g. F1, F2, etc.

```scheme
(key-event-F? event number) -> bool?
```
event : Event?
number : int?
       
### **mouse-event?**

Check if this event is a mouse event.

```scheme
(mouse-event event) -> bool?
```
event : Event?
### **event-mouse-kind**
Convert the mouse event kind into an integer representing the state.

```scheme
(event-mouse-kind event) -> (or int? #false)
```

event : Event?

This is the current mapping today:

```rust
match kind {
   helix_view::input::MouseEventKind::Down(MouseButton::Left) => 0,
   helix_view::input::MouseEventKind::Down(MouseButton::Right) => 1,
   helix_view::input::MouseEventKind::Down(MouseButton::Middle) => 2,
   helix_view::input::MouseEventKind::Up(MouseButton::Left) => 3,
   helix_view::input::MouseEventKind::Up(MouseButton::Right) => 4,
   helix_view::input::MouseEventKind::Up(MouseButton::Middle) => 5,
   helix_view::input::MouseEventKind::Drag(MouseButton::Left) => 6,
   helix_view::input::MouseEventKind::Drag(MouseButton::Right) => 7,
   helix_view::input::MouseEventKind::Drag(MouseButton::Middle) => 8,
   helix_view::input::MouseEventKind::Moved => 9,
   helix_view::input::MouseEventKind::ScrollDown => 10,
   helix_view::input::MouseEventKind::ScrollUp => 11,
   helix_view::input::MouseEventKind::ScrollLeft => 12,
   helix_view::input::MouseEventKind::ScrollRight => 13,
}
```

Any unhandled event that does not match this will return `#false`.
### **event-mouse-row**


Get the row from the mouse event, of #false if it isn't a mouse event.

```scheme
(event-mouse-row event) -> (or int? #false)
```

event : Event?
           
       
### **event-mouse-col**


Get the col from the mouse event, of #false if it isn't a mouse event.

```scheme
(event-mouse-row event) -> (or int? #false)
```

event : Event?
       
### **mouse-event-within-area?**
Check whether the given mouse event occurred within a given `Rect`.

```scheme
(mouse-event-within-area? event area) -> bool?
```

event : Event?
area : Rect?
       
### **key-event-escape?**

Check whether the given event is the key: escape

```scheme
(key-event-escape? event)
```
event: Event?
### **key-event-backspace?**

Check whether the given event is the key: backspace

```scheme
(key-event-backspace? event)
```
event: Event?
### **key-event-enter?**

Check whether the given event is the key: enter

```scheme
(key-event-enter? event)
```
event: Event?
### **key-event-left?**

Check whether the given event is the key: left

```scheme
(key-event-left? event)
```
event: Event?
### **key-event-right?**

Check whether the given event is the key: right

```scheme
(key-event-right? event)
```
event: Event?
### **key-event-up?**

Check whether the given event is the key: up

```scheme
(key-event-up? event)
```
event: Event?
### **key-event-down?**

Check whether the given event is the key: down

```scheme
(key-event-down? event)
```
event: Event?
### **key-event-home?**

Check whether the given event is the key: home

```scheme
(key-event-home? event)
```
event: Event?
### **key-event-page-up?**

Check whether the given event is the key: page-up

```scheme
(key-event-page-up? event)
```
event: Event?
### **key-event-page-down?**

Check whether the given event is the key: page-down

```scheme
(key-event-page-down? event)
```
event: Event?
### **key-event-tab?**

Check whether the given event is the key: tab

```scheme
(key-event-tab? event)
```
event: Event?
### **key-event-delete?**

Check whether the given event is the key: delete

```scheme
(key-event-delete? event)
```
event: Event?
### **key-event-insert?**

Check whether the given event is the key: insert

```scheme
(key-event-insert? event)
```
event: Event?
### **key-event-null?**

Check whether the given event is the key: null

```scheme
(key-event-null? event)
```
event: Event?
### **key-event-caps-lock?**

Check whether the given event is the key: caps-lock

```scheme
(key-event-caps-lock? event)
```
event: Event?
### **key-event-scroll-lock?**

Check whether the given event is the key: scroll-lock

```scheme
(key-event-scroll-lock? event)
```
event: Event?
### **key-event-num-lock?**

Check whether the given event is the key: num-lock

```scheme
(key-event-num-lock? event)
```
event: Event?
### **key-event-print-screen?**

Check whether the given event is the key: print-screen

```scheme
(key-event-print-screen? event)
```
event: Event?
### **key-event-pause?**

Check whether the given event is the key: pause

```scheme
(key-event-pause? event)
```
event: Event?
### **key-event-menu?**

Check whether the given event is the key: menu

```scheme
(key-event-menu? event)
```
event: Event?
### **key-event-keypad-begin?**

Check whether the given event is the key: keypad-begin

```scheme
(key-event-keypad-begin? event)
```
event: Event?
