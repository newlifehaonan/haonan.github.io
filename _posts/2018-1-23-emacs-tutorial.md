---
layout: post
title: "Emacs Tutorial"
date: 2018-1-23 11:30:20
tag:
- others
description: Emacs editor enter&exit, cursor manipulation, window split, buffer load & change command.
comments: true
---

# Emacs Command

**_Basic Command of Emacs text Editor_**

**_In the following text, `c` represent `Ctl` and `x` represent `ESC`_**

<hr />

## Basic Access

<hr />

* Open emacs: `emacs + filename`;

* get the tutorial: `c + h t`;

* exit emacs: `c + x` + `c + c`;

<hr />

## Cursor Manipulation

<hr />

* next page: `c + v`;

* previous page: `x + v`;

* next line: `c + n`;

* previous line: `c + p`;

* next letter: `c + f`;

* previous letter: `c + b`;

* next word: `x + f`;

* previous word: `x + b`;

* back to the begin: `x + <`;

* forward to the end: `x + >`;

* search: `c + s`;

* repeat command: `c + u`;

* stop a command: `c + g`;

<hr />

## Text Edit

<hr />

* delete a letter: `delete`,`c + d`;

* delete a word `x + delete`,`x + d`

* delete a line from cursor to the line end: `c+k`

* delete a user defined line: `c + spc` + `c + w`;

* cut and paste: `c + k` + `c + y`;

* undo: `c +x u`;

<hr />

## File and Windows

<hr />

* find file: `c + x` + `c + f`;

* save file: `c + x s `;

* open buffer list: `c + x` + `c + b`;

* switch buffer: `c + x b`；

* split the window: `c + x 2`;

* switch window: `c + x o`;

* follow the cursor in another window: `x + c + v`;

* find file and open in another window: `c + x 4` + `c + f`;
