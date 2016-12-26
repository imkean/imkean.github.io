---
layout: page
date: 2016-07-20
title: ctype
categories: clibrary
file_name: ctype.h
tags: [isalnum, isalpha, isblank, iscntrl, isdigit, isgraph, islower, isprint, ispunct, isspace, isupper, isxdigit, tolower, toupper]
---


## Character classification functions

**isalnum**

Check if character is alphanumeric

**isalpha**

Check if character is alphabetic

**isblank**

Check if character is blank

**iscntrl**

Check if character is a control character

**isdigit**

Check if character is decimal digit

**isgraph**

Check if character has graphical representation

**islower**

Check if character is lowercase letter

**isprint**

Check if character is printable

**ispunct**

Check if character is a punctuation character

**isspace**

Check if character is a white-space

**isupper**

Check if character is uppercase letter

**isxdigit**

Check if character is hexadecimal digit

## Character conversion functions

**tolower**

Convert uppercase letter to lowercase

**toupper**

Convert lowercase letter to uppercase

## ASCII map

<table class="table table-bordered">
<tbody><tr><th>ASCII values</th>
<th>characters</th>
<th>iscntrl</th>
<th>isblank</th>
<th>isspace</th>
<th>isupper</th>
<th>islower</th>
<th>isalpha</th>
<th>isdigit</th>
<th>isxdigit</th>
<th>isalnum</th>
<th>ispunct</th>
<th>isgraph</th>
<th>isprint</th>
</tr>
<tr><td>0x00 .. 0x08</td><td>NUL, (other control codes)</td>
<td>x</td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td></tr>
<tr><td>0x09</td><td>tab (<tt>'\t'</tt>)</td>
<td>x</td><td>x</td><td>x</td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td></tr>
<tr><td>0x0A .. 0x0D</td><td>(white-space control codes: <tt>'\f'</tt>,<tt>'\v'</tt>,<tt>'\n'</tt>,<tt>'\r'</tt>)</td>
<td>x</td><td> </td><td>x</td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td></tr>
<tr><td>0x0E .. 0x1F</td><td>(other control codes)</td>
<td>x</td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td></tr>
<tr><td>0x20</td><td>space (<tt>' '</tt>)</td>
<td> </td><td>x</td><td>x</td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td>x</td></tr>
<tr><td>0x21 .. 0x2F</td><td><tt>!"#$%&amp;'()*+,-./</tt></td>
<td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td>x</td><td>x</td><td>x</td></tr>
<tr><td>0x30 .. 0x39</td><td><tt>0123456789</tt></td>
<td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td>x</td><td>x</td><td>x</td><td> </td><td>x</td><td>x</td></tr>
<tr><td>0x3a .. 0x40</td><td><tt>:;&lt;=&gt;?@</tt></td>
<td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td>x</td><td>x</td><td>x</td></tr>
<tr><td>0x41 .. 0x46</td><td><tt>ABCDEF</tt></td>
<td> </td><td> </td><td> </td><td>x</td><td> </td><td>x</td><td> </td><td>x</td><td>x</td><td> </td><td>x</td><td>x</td></tr>
<tr><td>0x47 .. 0x5A</td><td><tt>GHIJKLMNOPQRSTUVWXYZ</tt></td>
<td> </td><td> </td><td> </td><td>x</td><td> </td><td>x</td><td> </td><td> </td><td>x</td><td> </td><td>x</td><td>x</td></tr>
<tr><td>0x5B .. 0x60</td><td><tt>[\]^_`</tt></td>
<td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td>x</td><td>x</td><td>x</td></tr>
<tr><td>0x61 .. 0x66</td><td><tt>abcdef</tt></td>
<td> </td><td> </td><td> </td><td> </td><td>x</td><td>x</td><td> </td><td>x</td><td>x</td><td> </td><td>x</td><td>x</td></tr>
<tr><td>0x67 .. 0x7A</td><td><tt>ghijklmnopqrstuvwxyz</tt></td>
<td> </td><td> </td><td> </td><td> </td><td>x</td><td>x</td><td> </td><td> </td><td>x</td><td> </td><td>x</td><td>x</td></tr>
<tr><td>0x7B .. 0x7E</td><td><tt>{|}~</tt></td>
<td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td>x</td><td>x</td><td>x</td></tr>
<tr><td>0x7F</td><td>(DEL)</td>
<td>x</td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td><td> </td></tr>
</tbody></table>
