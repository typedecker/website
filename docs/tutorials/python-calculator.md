---
title: Create Calculator app in Python with Flet
sidebar_label: Calculator app
slug: python-calculator
---

In this tutorial we will show you, step-by-step, how to create a Calculator app in Python using Flet framework and publish it as a desktop, mobile or web app. The app is [a simple console program](https://github.com/flet-dev/examples/blob/main/python/tutorials/calc/calc.py), yet it is a multi-platform application with similar to iPhone calculator app UI:

<img src="/img/docs/calc-tutorial/calc-app.gif" className="screenshot-40" />

You can find the live demo [here](https://gallery.flet.dev/calculator/).

In this tutorial, we will cover all of the basic concepts for creating a Flet app: building a page layout, adding controls, making reusable UI components, handling events, and publishing options.

The tutorial consists of the following steps:

import TOCInline from '@theme/TOCInline';

<TOCInline toc={toc} maxHeadingLevel={2} />

## Getting started with Flet

To create a multi-platform app in Python with Flet, you don't need to know HTML, CSS or JavaScript, but you do need a basic knowledge of Python and object-oriented programming.

Before you can create your first Flet app, you need to [setup your development environment](/docs/getting-started/), which requires Python 3.9 or above and `flet` package.

Once you have Flet installed, let's [create](/docs/getting-started/create-flet-app) a simple hello-world app.

Create `hello.py` with the following contents:

```python
import flet as ft

def main(page: ft.Page):
    page.add(ft.Text(value="Hello, world!"))

ft.app(main)
```

Run this app and you will see a new window with a greeting:

<img src="/img/docs/tutorial/todo-app-hello-world.png" className="screenshot-40" />

## Adding page controls

Now you are ready to create a calculator app.

To start, you'll need a [Text](/docs/controls/text) control for showing the result of calculation, and a few [ElevatedButtons](/docs/controls/elevatedbutton) with all the numbers and actions on them.

Create `calc.py` with the following contents:

```python
import flet as ft


def main(page: ft.Page):
    page.title = "Calc App"
    result = ft.Text(value="0")

    page.add(
        result,
        ft.ElevatedButton(text="AC"),
        ft.ElevatedButton(text="+/-"),
        ft.ElevatedButton(text="%"),
        ft.ElevatedButton(text="/"),
        ft.ElevatedButton(text="7"),
        ft.ElevatedButton(text="8"),
        ft.ElevatedButton(text="9"),
        ft.ElevatedButton(text="*"),
        ft.ElevatedButton(text="4"),
        ft.ElevatedButton(text="5"),
        ft.ElevatedButton(text="6"),
        ft.ElevatedButton(text="-"),
        ft.ElevatedButton(text="1"),
        ft.ElevatedButton(text="2"),
        ft.ElevatedButton(text="3"),
        ft.ElevatedButton(text="+"),
        ft.ElevatedButton(text="0"),
        ft.ElevatedButton(text="."),
        ft.ElevatedButton(text="="),
    )


ft.app(main)
```

Run the app and you should see a page like this:

<img src="/img/docs/calc-tutorial/calc-app-1.png" className="screenshot-10" />

## Building page layout

Now let's arrange the text and buttons in 6 horizontal [rows](/docs/controls/row).

Replace `calc.py` contents with the following:

```python
import flet as ft


def main(page: ft.Page):
    page.title = "Calc App"
    result = ft.Text(value="0")

    page.add(
        ft.Row(controls=[result]),
        ft.Row(
            controls=[
                ft.ElevatedButton(text="AC"),
                ft.ElevatedButton(text="+/-"),
                ft.ElevatedButton(text="%"),
                ft.ElevatedButton(text="/"),
            ]
        ),
        ft.Row(
            controls=[
                ft.ElevatedButton(text="7"),
                ft.ElevatedButton(text="8"),
                ft.ElevatedButton(text="9"),
                ft.ElevatedButton(text="*"),
            ]
        ),
        ft.Row(
            controls=[
                ft.ElevatedButton(text="4"),
                ft.ElevatedButton(text="5"),
                ft.ElevatedButton(text="6"),
                ft.ElevatedButton(text="-"),
            ]
        ),
        ft.Row(
            controls=[
                ft.ElevatedButton(text="1"),
                ft.ElevatedButton(text="2"),
                ft.ElevatedButton(text="3"),
                ft.ElevatedButton(text="+"),
            ]
        ),
        ft.Row(
             controls=[
                ft.ElevatedButton(text="0"),
                ft.ElevatedButton(text="."),
                ft.ElevatedButton(text="="),
            ]
        ),
    )


ft.app(main)
```

Run the app and you should see a page like this:

<img src="/img/docs/calc-tutorial/calc-app-2.png" className="screenshot-40" />

### Using Container for decoration

To add a black background with rounded border around the calculator, we will be using [Container](/docs/controls/container) control. Container may decorate only one control, so we will need to wrap all the 6 rows into a single vertical [Column](/docs/controls/container) that will be used as the container's `content`:
<img src="/img/docs/calc-tutorial/container-layout.svg" className="screenshot" />

Here is the code for adding the container to the page:

```python
    page.add(
        ft.Container(
            width=350,
            bgcolor=ft.Colors.BLACK,
            border_radius=ft.border_radius.all(20),
            padding=20,
            content=ft.Column(
                controls=
                    [] # Controls will the six rows with the text 
                       # and the calculator buttons.
            )
        )
    )
```

### Styled Controls

To complete the UI portion of the program, we need to update style for result text and buttons to look similar to iPhone calculator app.

For the result text, let's specify its  `color` and `size` properties:
```python
result = ft.Text(value="0", color=ft.Colors.WHITE, size=20)
```

For the buttons, if we look again at the UI we are aiming to achieve, there are 3 types of buttons:
1. **Digit Buttons**. They have dark grey background color and white text, size is the same for all.
2. **Action Buttons**.  They have orange background color and white text, size is the same for all except `0` button which is twice as large.
3. **Extra action buttons**. They have light grey background color and dark text, size is the same for all.

The buttons will be used multiple time in the program, so we will be creating
custom [Styled Controls](/docs/getting-started/custom-controls#styled-controls) to reuse the code.

Since all those types should inherit from `ElevatedButton` class and have common `text` and `expand` properties, let's create a parent `CalcButton` class:
```python
    class CalcButton(ft.ElevatedButton):
        def __init__(self, text, expand=1):
            super().__init__()
            self.text = text
            self.expand = expand
```

Now let's create child classes for all three types of buttons:

```python
    class DigitButton(CalcButton):
        def __init__(self, text, expand=1):
            CalcButton.__init__(self, text, expand)
            self.bgcolor = ft.Colors.WHITE24
            self.color = ft.Colors.WHITE

    class ActionButton(CalcButton):
        def __init__(self, text):
            CalcButton.__init__(self, text)
            self.bgcolor = ft.Colors.ORANGE
            self.color = ft.Colors.WHITE

    class ExtraActionButton(CalcButton):
        def __init__(self, text):
            CalcButton.__init__(self, text)
            self.bgcolor = ft.Colors.BLUE_GREY_100
            self.color = ft.Colors.BLACK
```

We will be using these new classes now to create rows of buttons in the Container:

```python
content=ft.Column(
                controls=[
                    ft.Row(controls=[result], alignment="end"),
                    ft.Row(
                        controls=[
                            ExtraActionButton(text="AC"),
                            ExtraActionButton(text="+/-"),
                            ExtraActionButton(text="%"),
                            ActionButton(text="/"),
                        ]
                    ),
                    ft.Row(
                        controls=[
                            DigitButton(text="7"),
                            DigitButton(text="8"),
                            DigitButton(text="9"),
                            ActionButton(text="*"),
                        ]
                    ),
                    ft.Row(
                        controls=[
                            DigitButton(text="4"),
                            DigitButton(text="5"),
                            DigitButton(text="6"),
                            ActionButton(text="-"),
                        ]
                    ),
                    ft.Row(
                        controls=[
                            DigitButton(text="1"),
                            DigitButton(text="2"),
                            DigitButton(text="3"),
                            ActionButton(text="+"),
                        ]
                    ),
                    ft.Row(
                        controls=[
                            DigitButton(text="0", expand=2),
                            DigitButton(text="."),
                            ActionButton(text="="),
                        ]
                    ),
                ]
            ),
```

Since the program is too long now to be fully included in this tutorial, copy the entire code for this step from [here](https://github.com/flet-dev/examples/blob/main/python/tutorials/calc/calc3.py). Run the app and you should see a page like this:
<img src="/img/docs/calc-tutorial/calc-app.png" className="screenshot-40" />

Just what we wanted!

## Reusable UI components

While you can continue writing your app in the `main` function, the best practice would be to create a [reusable UI component](/docs/getting-started/custom-controls#composite-controls). 

Imagine you are working on an app header, a side menu, or UI that will be a part of a larger project (for example, at Flet we will be using this Calculator app in a bigger "Gallery" app that will show all the examples for Flet framework). 

Even if you can't think of such uses right now, we still recommend creating all your Flet apps with composability and reusability in mind. 

To make a reusable Calc app component, we are going to encapsulate its state and presentation logic in a separate `CalculatorApp` class. Copy the entire code for this step from [here](https://github.com/flet-dev/examples/blob/main/python/tutorials/calc/calc4.py).

:::note Try something
Try adding two `CalculatorApp` components to the page:

```python
# create application instance
calc1 = CalculatorApp()
calc2 = CalculatorApp()

# add application's root control to the page
page.add(calc1, calc2)
```

:::

## Handling events

Now let's make the calculator do its job. We will be using the same event handler for all the buttons and use `data` property to differentiate between the actions depending on the button clicked. For `CalcButton` class, let's specify `on_click=button_clicked` event and set `data` property equal to button's text:

```python
class CalcButton(ft.ElevatedButton):
    def __init__(self, text, button_clicked, expand=1):
        super().__init__()
        self.text = text
        self.expand = expand
        self.on_click = button_clicked
        self.data = text
```

We will define `button_click` method in `CalculatorClass` and pass it to each button. Below is `on_click` event handler that will reset the Text value when "AC" button is clicked:

```python
def button_clicked(self, e):
    if e.control.data == "AC":
        self.result.value = "0"
```

With similar approach, `button_click` method will handle different calculator actions depending on `data` property for each button. Copy the entire code for this step from [here](https://github.com/flet-dev/examples/blob/main/python/tutorials/calc/calc.py).

Run the app and see it in the action:
<img src="/img/docs/calc-tutorial/calc-app.gif" className="screenshot-40" />

## Publishing your app

Congratulations! You have created your Calculator app with Flet, and it looks awesome! Now it's time to share your app with the world!

Flet Python app and all its dependencies can be packaged into a standalone executable a package for distribution using `flet build` command.

[Follow these instructions](/docs/publish) to package your Calculator app into a desktop executable, mobile app bundle or web app.

## Summary

In this tutorial you have learned how to:

* [Create](/docs/getting-started/create-flet-app) a simple Flet app;
* Work with [Reusable UI components](/docs/getting-started/custom-controls);
* Design UI layout using `Column`, `Row` and `Container` controls;
* Handle events;
* [Publish](/docs/publish/) your Flet app to multiple platforms;

For further reading you can explore [controls](/docs/controls) and [examples repository](https://github.com/flet-dev/examples/tree/main/python).

We would love to hear your feedback! Please drop us an [email](mailto:hello@flet.dev), join the discussion on [Discord](https://discord.gg/dzWXP8SHG8).
