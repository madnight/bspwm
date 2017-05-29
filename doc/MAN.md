Name
====

bspwm - Binary space partitioning window manager

Synopsis
========

**bspwm** \[**-h**|**-v**|**-c** *CONFIG\_PATH*\]

**bspc** *DOMAIN* \[*SELECTOR*\] *COMMANDS*

**bspc** *COMMAND* \[*OPTIONS*\] \[*ARGUMENTS*\]

Description
===========

**bspwm** is a tiling window manager that represents windows as the leaves of a full binary tree.

It is controlled and configured via **bspc**.

Options
=======

**-h**  
Print the synopsis and exit.

**-v**  
Print the version and exit.

**-c** *CONFIG\_PATH*  
Use the given configuration file.

Common Definitions
==================

    DIR         := north | west | south | east
    CYCLE_DIR   := next | prev

Selectors
=========

Selectors are used to select a target node, desktop, or monitor. A selector can either describe the target relatively or name it globally.

Selectors consist of an optional reference, a descriptor and any number of non-conflicting modifiers as follows:

    [REFERENCE#]DESCRIPTOR(.MODIFIER)*

The relative targets are computed in relation to the given reference (the default reference value is **focused**).

An exclamation mark can be prepended to any modifier in order to reverse its meaning.

Node
----

Select a node.

    NODE_SEL := [NODE_SEL#](DIR|CYCLE_DIR|PATH|last|older|newer|focused|pointed|biggest|<node_id>)[.[!]focused][.[!]automatic][.[!]local][.[!]active][.[!]leaf][.[!]window][.[!]STATE][.[!]FLAG][.[!]LAYER][.[!]same_class][.[!]descendant_of][.[!]ancestor_of]

    STATE := tiled|pseudo_tiled|floating|fullscreen

    FLAG := hidden|sticky|private|locked|urgent

    LAYER := below|normal|above

    PATH := @[DESKTOP_SEL:][[/]JUMP](/JUMP)*

    JUMP := first|1|second|2|brother|parent|DIR

### Descriptors

*DIR*  
Selects the window in the given (spacial) direction relative to the reference node.

*CYCLE\_DIR*  
Selects the window in the given (cyclic) direction relative to the reference node.

*PATH*  
Selects the node at the given path.

last  
Selects the previously focused node relative to the reference node.

older  
Selects the node older than the reference node in the history.

newer  
Selects the node newer than the reference node in the history.

focused  
Selects the currently focused node.

pointed  
Selects the window under the pointer.

biggest  
Selects the biggest window.

&lt;node\_id&gt;  
Selects the node with the given ID.

### Path Jumps

The initial node is the focused node (or the root if the path starts with */*) of the reference desktop (or the selected desktop if the path has a *DESKTOP\_SEL* prefix).

1|first  
Jumps to the first child.

2|second  
Jumps to the second child.

brother  
Jumps to the brother node.

parent  
Jumps to the parent node.

*DIR*  
Jumps to the node holding the edge in the given direction.

### Modifiers

\[!\]focused  
Only consider focused or unfocused nodes.

\[!\]automatic  
Only consider nodes in automatic or manual insertion mode. See also **--presel-dir** under **Node** in the **DOMAINS** section below.

\[!\]local  
Only consider nodes in or not in the reference desktop.

\[!\]active  
Only consider nodes in or not in the active desktop of their monitor.

\[!\]leaf  
Only consider leaves or internal nodes.

\[!\]window  
Only consider nodes that hold or don’t hold a window.

\[!\](tiled|pseudo\_tiled|floating|fullscreen)  
Only consider windows in or not in the given state.

\[!\]same\_class  
Only consider windows that have or don’t have the same class as the reference window.

\[!\]descendant\_of  
Only consider nodes that are or aren’t descendants of the reference node.

\[!\]ancestor\_of  
Only consider nodes that are or aren’t ancestors of the reference node.

\[!\](hidden|sticky|private|locked|urgent)  
Only consider windows that have or don’t have the given flag set.

\[!\](below|normal|above)  
Only consider windows in or not in the given layer.

Desktop
-------

Select a desktop.

    DESKTOP_SEL := [DESKTOP_SEL#](CYCLE_DIR|last|older|newer|[MONITOR_SEL:](focused|^<n>)|<desktop_id>|<desktop_name>)[.[!]occupied][.[!]focused][.[!]urgent][.[!]local]

### Descriptors

*CYCLE\_DIR*  
Selects the desktop in the given direction relative to the reference desktop.

last  
Selects the previously focused desktop relative to the reference desktop.

older  
Selects the desktop older than the reference desktop in the history.

newer  
Selects the desktop newer than the reference desktop in the history.

focused  
Selects the currently focused desktop.

^&lt;n&gt;  
Selects the nth desktop.

&lt;desktop\_id&gt;  
Selects the desktop with the given ID.

&lt;desktop\_name&gt;  
Selects the desktop with the given name.

### Modifiers

\[!\]occupied  
Only consider occupied or free desktops.

\[!\]focused  
Only consider focused or unfocused desktops.

\[!\]urgent  
Only consider urgent or non urgent desktops.

\[!\]local  
Only consider desktops inside or outside of the reference monitor.

Monitor
-------

Select a monitor.

    MONITOR_SEL := [MONITOR_SEL#](DIR|CYCLE_DIR|last|older|newer|focused|primary|^<n>|<monitor_id>|<monitor_name>)[.[!]occupied][.[!]focused]

### Descriptors

*DIR*  
Selects the monitor in the given (spacial) direction relative to the reference monitor.

*CYCLE\_DIR*  
Selects the monitor in the given (cyclic) direction relative to the reference monitor.

last  
Selects the previously focused monitor relative to the reference monitor.

older  
Selects the monitor older than the reference monitor in the history.

newer  
Selects the monitor newer than the reference monitor in the history.

focused  
Selects the currently focused monitor.

primary  
Selects the primary monitor.

^&lt;n&gt;  
Selects the nth monitor.

&lt;monitor\_id&gt;  
Selects the monitor with the given ID.

&lt;monitor\_name&gt;  
Selects the monitor with the given name.

### Modifiers

\[!\]occupied  
Only consider monitors where the focused desktop is occupied or free.

\[!\]focused  
Only consider focused or unfocused monitors.

Window States
=============

tiled  
Its size and position are determined by the splitting type and ratio of each node of its path in the window tree.

pseudo\_tiled  
Has an unrestricted size while being centered in its tiling space.

floating  
Can be moved/resized freely. Although it doesn’t occupy any tiling space, it is still part of the window tree.

fullscreen  
Fills its monitor rectangle and has no borders. It is send in the ABOVE layer by default.

Node Flags
==========

hidden  
Is hidden and doesn’t occupy any tiling space.

sticky  
Stays in the focused desktop of its monitor.

private  
Tries to keep the same tiling position/size.

locked  
Ignores the **node --close** message.

urgent  
Has its urgency hint set. This flag is set externally.

Stacking Layers
===============

There’s three stacking layers: BELOW, NORMAL and ABOVE.

In each layer, the window are orderered as follow: tiled & pseudo-tiled &lt; fullscreen &lt; floating.

Domains
=======

Node
----

### General Syntax

node \[*NODE\_SEL*\] *COMMANDS*

If *NODE\_SEL* is omitted, **focused** is assumed.

### Commands

**-f**, **--focus** \[*NODE\_SEL*\]  
Focus the selected or given node.

**-a**, **--activate** \[*NODE\_SEL*\]  
Activate the selected or given node.

**-d**, **--to-desktop** *DESKTOP\_SEL*  
Send the selected node to the given desktop.

**-m**, **--to-monitor** *MONITOR\_SEL*  
Send the selected node to the given monitor.

**-n**, **--to-node** *NODE\_SEL*  
Transplant the selected node to the given node.

**-s**, **--swap** *NODE\_SEL*  
Swap the selected node with the given node.

**-p**, **--presel-dir** \[~\]*DIR*|cancel  
Preselect the splitting area of the selected node (or cancel the preselection). If **~** is prepended to *DIR* and the current preselection direction matches *DIR*, then the argument is interpreted as **cancel**. A node with a preselected area is said to be in "manual insertion mode".

**-o**, **--presel-ratio** *RATIO*  
Set the splitting ratio of the preselection area.

**-v**, **--move** *dx* *dy*  
Move the selected window by *dx* pixels horizontally and *dy* pixels vertically.

**-z**, **--resize** top|left|bottom|right|top\_left|top\_right|bottom\_right|bottom\_left *dx* *dy*  
Resize the selected window by moving the given handle by *dx* pixels horizontally and *dy* pixels vertically.

**-r**, **--ratio** *RATIO*|(+|-)(*PIXELS*|*FRACTION*)  
Set the splitting ratio of the selected node (0 &lt; *RATIO* &lt; 1).

**-R**, **--rotate** *90|270|180*  
Rotate the tree rooted at the selected node.

**-F**, **--flip** *horizontal|vertical*  
Flip the the tree rooted at selected node.

**-E**, **--equalize**  
Reset the split ratios of the tree rooted at the selected node to their default value.

**-B**, **--balance**  
Adjust the split ratios of the tree rooted at the selected node so that all windows occupy the same area.

**-C**, **--circulate** forward|backward  
Circulate the windows of the tree rooted at the selected node.

**-t**, **--state** \[~\](tiled|pseudo\_tiled|floating|fullscreen)  
Set the state of the selected window. If **~** is present and the current state matches the given state, then the argument is interpreted as the last state.

**-g**, **--flag** hidden|sticky|private|locked\[=on|off\]  
Set or toggle the given flag for the selected node.

**-l**, **--layer** below|normal|above  
Set the stacking layer of the selected window.

**-i**, **--insert-receptacle**  
Insert a receptacle node at the selected node.

**-c**, **--close**  
Close the windows rooted at the selected node.

**-k**, **--kill**  
Kill the windows rooted at the selected node.

Desktop
-------

### General Syntax

desktop \[*DESKTOP\_SEL*\] *COMMANDS*

If *DESKTOP\_SEL* is omitted, **focused** is assumed.

### COMMANDS

**-f**, **--focus** \[*DESKTOP\_SEL*\]  
Focus the selected or given desktop.

**-a**, **--activate** \[*DESKTOP\_SEL*\]  
Activate the selected or given desktop.

**-m**, **--to-monitor** *MONITOR\_SEL*  
Send the selected desktop to the given monitor.

**-l**, **--layout** *CYCLE\_DIR*|monocle|tiled  
Set or cycle the layout of the selected desktop.

**-n**, **--rename** &lt;new\_name&gt;  
Rename the selected desktop.

**-s**, **--swap** *DESKTOP\_SEL*  
Swap the selected desktop with the given desktop.

**-b**, **--bubble** *CYCLE\_DIR*  
Bubble the selected desktop in the given direction.

**-r**, **--remove**  
Remove the selected desktop.

Monitor
-------

### General Syntax

monitor \[*MONITOR\_SEL*\] *COMMANDS*

If *MONITOR\_SEL* is omitted, **focused** is assumed.

### Commands

**-f**, **--focus** \[*MONITOR\_SEL*\]  
Focus the selected or given monitor.

**-s**, **--swap** *MONITOR\_SEL*  
Swap the selected monitor with the given monitor.

**-a**, **--add-desktops** &lt;name&gt;…  
Create desktops with the given names in the selected monitor.

**-o**, **--reorder-desktops** &lt;name&gt;…  
Reorder the desktops of the selected monitor to match the given order.

**-d**, **--reset-desktops** &lt;name&gt;…  
Rename, add or remove desktops depending on whether the number of given names is equal, superior or inferior to the number of existing desktops.

**-g**, **--rectangle** WxH+X+Y  
Set the rectangle of the selected monitor.

**-n**, **--rename** &lt;new\_name&gt;  
Rename the selected monitor.

**-r**, **--remove**  
Remove the selected monitor.

Query
-----

### General Syntax

query *COMMANDS* \[*OPTIONS*\]

### Commands

The optional selectors are references.

**-N**, **--nodes** \[*NODE\_SEL*\]  
List the IDs of the matching nodes.

**-D**, **--desktops** \[*DESKTOP\_SEL*\]  
List the IDs (or names) of the matching desktops.

**-M**, **--monitors** \[*MONITOR\_SEL*\]  
List the IDs (or names) of the matching monitors.

**-T**, **--tree**  
Print a JSON representation of the matching item.

### Options

**-m**,**--monitor** \[*MONITOR\_SEL*\]; **-d**,**--desktop** \[*DESKTOP\_SEL*\]; **-n**, **--node** \[*NODE\_SEL*\]  
Constrain matches to the selected monitor, desktop or node. The descriptor can be omitted for *-M*, *-D* and *-N*.

**--names**  
Print names instead of IDs.

Wm
--

### General Syntax

wm *COMMANDS*

### Commands

**-d**, **--dump-state**  
Dump the current world state on standard output.

**-l**, **--load-state** &lt;file\_path&gt;  
Load a world state from the given file.

**-a**, **--add-monitor** &lt;name&gt; WxH+X+Y  
Add a monitor for the given name and rectangle.

**-o**, **--adopt-orphans**  
Manage all the unmanaged windows remaining from a previous session.

**-h**, **--record-history** on|off  
Enable or disable the recording of node focus history.

**-g**, **--get-status**  
Print the current status information.

Rule
----

### General Syntax

rule *COMMANDS*

### Commands

**-a**, **--add** (&lt;class\_name&gt;|\*)\[:(&lt;instance\_name&gt;|\*)\] \[**-o**|**--one-shot**\] \[monitor=MONITOR\_SEL|desktop=DESKTOP\_SEL|node=NODE\_SEL\] \[state=STATE\] \[layer=LAYER\] \[split\_dir=DIR\] \[split\_ratio=RATIO\] \[(hidden|sticky|private|locked|center|follow|manage|focus|border)=(on|off)\]  
Create a new rule.

**-r**, **--remove** ^&lt;n&gt;|head|tail|(&lt;class\_name&gt;|\*)\[:(&lt;instance\_name&gt;|\*)\]…  
Remove the given rules.

**-l**, **--list**  
List the rules.

Config
------

### General Syntax

config \[-m *MONITOR\_SEL*|-d *DESKTOP\_SEL*|-n *NODE\_SEL*\] &lt;setting&gt; \[&lt;value&gt;\]  
Get or set the value of &lt;setting&gt;.

Subscribe
---------

### General Syntax

subscribe (all|report|monitor|desktop|node|…)\*  
Continuously print status information. See the **EVENTS** section for the detailed description of each event.

Quit
----

### General Syntax

quit \[&lt;status&gt;\]  
Quit with an optional exit status.

Exit Codes
==========

If the server can’t handle a message, **bspc** will return with a non-zero exit code.

Settings
========

Colors are in the form *\#RRGGBB*, booleans are *true*, *on*, *false* or *off*.

All the boolean settings are *false* by default unless stated otherwise.

Global Settings
---------------

*normal\_border\_color*  
Color of the border of an unfocused window.

*active\_border\_color*  
Color of the border of a focused window of an unfocused monitor.

*focused\_border\_color*  
Color of the border of a focused window of a focused monitor.

*presel\_feedback\_color*  
Color of the **node --presel-{dir,ratio}** message feedback area.

*split\_ratio*  
Default split ratio.

*status\_prefix*  
Prefix prepended to each of the status lines.

*external\_rules\_command*  
External command used to retrieve rule consequences. The command will receive the following arguments: window ID, class and instance names, monitor, desktop and node selectors. The output of that command must have the following format: **key1=value1 key2=value2 …** (the valid key/value pairs are given in the description of the *rule* command).

*initial\_polarity*  
On which child should a new window be attached when adding a window on a single window tree in automatic mode. Accept the following values: **first\_child**, **second\_child**.

*borderless\_monocle*  
Remove borders of tiled windows for the **monocle** desktop layout.

*gapless\_monocle*  
Remove gaps of tiled windows for the **monocle** desktop layout.

*paddingless\_monocle*  
Remove padding space for the **monocle** desktop layout.

*single\_monocle*  
Set the desktop layout to **monocle** if there’s only one tiled window in the tree.

*pointer\_motion\_interval*  
The minimum interval, in milliseconds, between two motion notify events.

*pointer\_modifier*  
Keyboard modifier used for moving or resizing windows. Accept the following values: **shift**, **control**, **lock**, **mod1**, **mod2**, **mod3**, **mod4**, **mod5**.

*pointer\_action1*; *pointer\_action2*; *pointer\_action3*  
Action performed when pressing *pointer\_modifier* + *button&lt;n&gt;*. Accept the following values: **move**, **resize\_side**, **resize\_corner**, **focus**, **none**.

*click\_to\_focus*  
Focus a window (or a monitor) by clicking it.

*swallow\_first\_click*  
Don’t replay the click that makes a window focused when *click\_to\_focus* is set.

*focus\_follows\_pointer*  
Focus the window under the pointer.

*pointer\_follows\_focus*  
When focusing a window, put the pointer at its center.

*pointer\_follows\_monitor*  
When focusing a monitor, put the pointer at its center.

*ignore\_ewmh\_focus*  
Ignore EWMH focus requests coming from applications.

*center\_pseudo\_tiled*  
Center pseudo tiled windows into their tiling rectangles. Defaults to *true*.

*honor\_size\_hints*  
Apply ICCCM window size hints.

*remove\_disabled\_monitors*  
Consider disabled monitors as disconnected.

*remove\_unplugged\_monitors*  
Remove unplugged monitors.

*merge\_overlapping\_monitors*  
Merge overlapping monitors (the bigger remains).

Monitor and Desktop Settings
----------------------------

*top\_padding*; *right\_padding*; *bottom\_padding*; *left\_padding*  
Padding space added at the sides of the monitor or desktop.

Desktop Settings
----------------

*window\_gap*  
Size of the gap that separates windows.

Node Settings
-------------

*border\_width*  
Window border width.

Pointer Bindings
================

*button1*  
Focus the window under the pointer if *click\_to\_focus* is set.

*pointer\_modifier* + *button1*  
Move the window under the pointer.

*pointer\_modifier* + *button2*  
Resize the window under the pointer by dragging the nearest side.

*pointer\_modifier* + *button3*  
Resize the window under the pointer by dragging the nearest corner.

The behavior of *pointer\_modifier* + *button&lt;n&gt;* can be modified through the *pointer\_action&lt;n&gt;* setting.

Events
======

*report*  
See the next section for the description of the format.

*monitor\_add &lt;monitor\_id&gt; &lt;monitor\_name&gt; &lt;monitor\_geometry&gt;*  
A monitor is added.

*monitor\_rename &lt;monitor\_id&gt; &lt;old\_name&gt; &lt;new\_name&gt;*  
A monitor is renamed.

*monitor\_remove &lt;monitor\_id&gt;*  
A monitor is removed.

*monitor\_swap &lt;src\_monitor\_id&gt; &lt;dst\_monitor\_id&gt;*  
A monitor is swapped.

*monitor\_focus &lt;monitor\_id&gt;*  
A monitor is focused.

*monitor\_geometry &lt;monitor\_id&gt; &lt;monitor\_geometry&gt;*  
The geometry of a monitor changed.

*desktop\_add &lt;monitor\_id&gt; &lt;desktop\_id&gt; &lt;desktop\_name&gt;*  
A desktop is added.

*desktop\_rename &lt;monitor\_id&gt; &lt;desktop\_id&gt; &lt;old\_name&gt; &lt;new\_name&gt;*  
A desktop is renamed.

*desktop\_remove &lt;monitor\_id&gt; &lt;desktop\_id&gt;*  
A desktop is removed.

*desktop\_swap &lt;src\_monitor\_id&gt; &lt;src\_desktop\_id&gt; &lt;dst\_monitor\_id&gt; &lt;dst\_desktop\_id&gt;*  
A desktop is swapped.

*desktop\_transfer &lt;src\_monitor\_id&gt; &lt;src\_desktop\_id&gt; &lt;dst\_monitor\_id&gt;*  
A desktop is transferred.

*desktop\_focus &lt;monitor\_id&gt; &lt;desktop\_id&gt;*  
A desktop is focused.

*desktop\_activate &lt;monitor\_id&gt; &lt;desktop\_id&gt;*  
A desktop is activated.

*desktop\_layout &lt;monitor\_id&gt; &lt;desktop\_id&gt; tiled|monocle*  
The layout of a desktop changed.

*node\_manage &lt;monitor\_id&gt; &lt;desktop\_id&gt; &lt;node\_id&gt; &lt;ip\_id&gt;*  
A window is managed.

*node\_unmanage &lt;monitor\_id&gt; &lt;desktop\_id&gt; &lt;node\_id&gt;*  
A window is unmanaged.

*node\_swap &lt;src\_monitor\_id&gt; &lt;src\_desktop\_id&gt; &lt;src\_node\_id&gt; &lt;dst\_monitor\_id&gt; &lt;dst\_desktop\_id&gt; &lt;dst\_node\_id&gt;*  
A node is swapped.

*node\_transfer &lt;src\_monitor\_id&gt; &lt;src\_desktop\_id&gt; &lt;src\_node\_id&gt; &lt;dst\_monitor\_id&gt; &lt;dst\_desktop\_id&gt; &lt;dst\_node\_id&gt;*  
A node is transferred.

*node\_focus &lt;monitor\_id&gt; &lt;desktop\_id&gt; &lt;node\_id&gt;*  
A node is focused.

*node\_activate &lt;monitor\_id&gt; &lt;desktop\_id&gt; &lt;node\_id&gt;*  
A node is activated.

*node\_presel &lt;monitor\_id&gt; &lt;desktop\_id&gt; &lt;node\_id&gt; (dir DIR|ratio RATIO|cancel)*  
A node is preselected.

*node\_stack &lt;node\_id\_1&gt; below|above &lt;node\_id\_2&gt;*  
A node is stacked below or above another node.

*node\_geometry &lt;monitor\_id&gt; &lt;desktop\_id&gt; &lt;node\_id&gt; &lt;node\_geometry&gt;*  
The geometry of a window changed.

*node\_state &lt;monitor\_id&gt; &lt;desktop\_id&gt; &lt;node\_id&gt; tiled|pseudo\_tiled|floating|fullscreen on|off*  
The state of a window changed.

*node\_flag &lt;monitor\_id&gt; &lt;desktop\_id&gt; &lt;node\_id&gt; hidden|sticky|private|locked|urgent on|off*  
One of the flags of a node changed.

*node\_layer &lt;monitor\_id&gt; &lt;desktop\_id&gt; &lt;node\_id&gt; below|normal|above*  
The layer of a window changed.

*pointer\_action &lt;monitor\_id&gt; &lt;desktop\_id&gt; &lt;node\_id&gt; move|resize\_corner|resize\_side begin|end*  
A pointer action occured.

Please note that **bspwm** initializes monitors before it reads messages on its socket, therefore the initial monitor events can’t be received.

Report Format
=============

Each report event message is composed of items separated by colons.

Each item has the form *&lt;type&gt;&lt;value&gt;* where *&lt;type&gt;* is the first character of the item.

*M&lt;monitor\_name&gt;*  
Focused monitor.

*m&lt;monitor\_name&gt;*  
Unfocused monitor.

*O&lt;desktop\_name&gt;*  
Occupied focused desktop.

*o&lt;desktop\_name&gt;*  
Occupied unfocused desktop.

*F&lt;desktop\_name&gt;*  
Free focused desktop.

*f&lt;desktop\_name&gt;*  
Free unfocused desktop.

*U&lt;desktop\_name&gt;*  
Urgent focused desktop.

*u&lt;desktop\_name&gt;*  
Urgent unfocused desktop.

*L(T|M)*  
Layout of the focused desktop of a monitor.

*T(T|P|F|=|@)*  
State of the focused node of a focused desktop.

*G(S?P?L?)*  
Active flags of the focused node of a focused desktop.

Environment Variables
=====================

*BSPWM\_SOCKET*  
The path of the socket used for the communication between **bspc** and **bspwm**. If it isn’t defined, then the following path is used: */tmp/bspwm&lt;host\_name&gt;\_&lt;display\_number&gt;\_&lt;screen\_number&gt;-socket*.

Contributors
============

-   Steven Allen &lt;steven at stebalien.com&gt;

-   Thomas Adam &lt;thomas at xteddy.org&gt;

-   Ivan Kanakarakis &lt;ivan.kanak at gmail.com&gt;

Author
======

Bastien Dejean &lt;nihilhill at gmail.com&gt;
