---
Title: What is event bubbling and how does it work
---

# What is event bubbling and how does it work

Event bubbling is part of the process where a browser captures user interaction, such as a click event or a keyboard event.

The DOM is a nested data structure where the window is the root, the document is its child, and every node on the web page are children underneath them. So when you click an element on the page, you have also clicked all of its parent elements as well. The browser then determines which element you intended to click.

There are three phases of a browser event:

1.  **Capture**: The event starts at the window and traverses through the DOM until it reaches the most deeply nested element that you clicked.
2.  **Target**: The event reaches the target element.
3.  **Bubbing**: The event bubbles up from this target and returns to the window to denote the completion and handling of the event.

The bubbling phase is when we actually execute the callbacks based on an event, such as an `onclick`, so this is typically the phase we're most concerned about.

Bugs can arise from event bubbling when we have an event handler on a parent of the intended target node. The target will execute its callback, and then the parent will execute its callback as well unless we stop the event from bubbling up to it.

The browser may also have some default intended behavior when a certain event is triggered on an element, and we may wish to prevent that as well.

To block default behavior or stop an event from being handled by parent nodes, we have the following functions:

-   `preventDefault()`: Prevent the browser from executing its default behavior from an event. For example, clicking a checkbox and executing `preventDefault()` will stop it from checking and unchecking.
-   `stopPropagation()`: Stop the propagation of an event from continuing in either the capture or bubbling phase.
-   `stopImmediatePropagation()`: Stop the propagation of an event from continuing in either the capture or bubbling phase AND stop any further events that are being handled on the current element.

![](https://skilled.dev/images/eventbubbling.png)

Credit: javascript.info

# ---

Tags: #coding #interview

Topics: [[javascript]] 

