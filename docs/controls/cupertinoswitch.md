---
title: CupertinoSwitch
sidebar_label: CupertinoSwitch
---

An iOS-style switch.

Used to toggle the on/off state of a single setting.

A toggle represents a physical switch that allows someone to choose between two mutually exclusive options.

For example, "On/Off", "Show/Hide". Choosing an option should produce an immediate result.

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

## Examples

[Live example](https://flet-controls-gallery.fly.dev/input/cupertinoswitch)

### CupertinoSwitch and adaptive Switch


```python reference
https://github.com/flet-dev/examples/blob/example-polishing/python/controls/cupertino/cupertino-input-and-selections/cupertino-switch-example.py
```


<img src="/img/docs/controls/cupertinoswitch/cupertino-switch.gif" className="screenshot-70"/>

## Properties

### `active_color`

The [color](/docs/reference/colors) to use for the track when the switch is on.

### `autofocus`

True if the control will be selected as the initial focus. If there is more than one control on a page with autofocus set, then the first one added to the page will get focus.

### `focus_color`

The [color](/docs/reference/colors) to use for the focus highlight for keyboard interactions.

### `label`

The clickable label to display on the right of the switch.

### `label_position`

The position of the label relative to the switch.

Value is of type [`LabelPosition`](/docs/reference/types/labelposition) and defaults to `LabelPosition.RIGHT`.

### `off_label_color`

The [color](/docs/reference/colors) to use for the accessibility label when the switch is off.

### `on_label_color`

The [color](/docs/reference/colors) to use for the accessibility label when the switch is on.

Defaults to `cupertino_colors.WHITE`.

### `thumb_color`

The [color](/docs/reference/colors) of the switch's thumb.

### `track_color`

The [color](/docs/reference/colors) to use for the track when the switch is off.

### `value`

Current value of the switch.

## Events

### `on_blur`

Fires when the control has lost focus.

### `on_change`

Fires when the state of the switch is changed.

### `on_focus`

Fires when the control has received focus.