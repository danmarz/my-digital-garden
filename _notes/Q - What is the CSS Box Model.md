---
Title: What is the CSS Box Model
---

# What is the CSS Box Model

When laying out a web page, all the elements are treated as rectangles which we call boxes. CSS determines the position, size, and style of a box.

A box is comprised of `margin`, `border`, `padding`, and content.

-   `margin`: The outermost area of a box. It enforces the distance (empty space) a box wants to have from its surrounding neighbors
-   `border`: The border is the outer edge of the element and sits inside the margin.
-   `padding`: The empty space inside of an element between the edge (`border`) and the content of the element.
-   content: This contains the real content of an element/box which is the text, image, video, etc.

![](https://skilled.dev/images/boxmodel.png)

The `box-sizing` property is used to indicate how we want to determine the size of a given box. Web development primarily uses `box-sizing: border-box;` where the `border` and `padding` are included in an element's `width` and `height` size.
# ---

Tags: #coding #interview

Topics: [[Interview Qs]]

