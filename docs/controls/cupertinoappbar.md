---
title: CupertinoAppBar
sidebar_label: CupertinoAppBar
---

An iOS-styled application bar.

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

## Examples

[Live example](https://flet-controls-gallery.fly.dev/navigation/cupertinoappbar)

### Basic Example


```python reference
https://github.com/flet-dev/examples/blob/example-polishing/python/controls/cupertino/cupertino-navigation/cupertino-appbar-example.py
```



<img src="/img/docs/controls/cupertino-appbar/cupertino-appbar.png" className="screenshot-40"/>

## Properties

### `automatically_imply_leading`

Controls whether we should try to imply the leading control if None.

If `True` and `leading` is null, automatically try to deduce what the leading widget should be. 
If `False` and `leading` is None, leading space is given to title. If leading widget is not None, this parameter has no effect.

### `automatically_imply_middle`

Controls whether we should try to imply the middle control if None.

If `True` and `middle` is null, automatically fill in a Text control with the current route's title. If middle control
is not None, this parameter has no effect.

### `bgcolor`

The fill [color](/docs/reference/colors) to use for an AppBar. Default color is defined by current theme.

### `border`

The border of the app bar.
By default, a single pixel bottom border side is rendered.

Value is of type [`Border`](/docs/reference/types/border).

### `elevation`

This property controls the size of the shadow below the app bar.

Defaults to `4`.

### `leading`

A `Control` to display at the start of this app bar. Typically the leading control is an [`Icon`](/docs/controls/icon) or an [`IconButton`](/docs/controls/iconbutton).

If `None` and `automatically_imply_leading = True`, an appropriate button will be automatically created.

### `middle`

A `Control` to display in the middle of this app bar. Typically a [`Text`](/docs/controls/text) or a segmented control.

### `padding`

Defines the padding for the contents of the app bar.

Padding is an instance of [`Padding`](/docs/reference/types/padding) class.

If `None`, the app bar will adopt the following defaults:

- vertically, contents will be sized to the same height as the app bar itself minus the status bar.
- horizontally, padding will be `16` pixels according to iOS specifications unless the leading widget is an automatically inserted back button, in which case the padding will be `0`.

Vertical padding won't change the height of the app bar.

### `trailing`

A Control to place at the end of the app bar. Normally additional actions taken on the page such as a search or edit function.
