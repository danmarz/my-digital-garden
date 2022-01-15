---
Title: Explain event delegation
---

# Explain event delegation

Event delegation is when you attach an event listener to a parent instead of its children.

For example, imagine you had a list `<ul>` with 10000 children `<li>` nodes. If you were to attach an `onclick` handler to each `<li>`, it could require the browser to use significant resources to create, maintain, and remove all of these.

On the other hand, you could attach one `onclick` handler to the `<ul>` element. Then if you were to click an `<li>`, you could recognize and handle this event during the bubbling phase.
# ---

Tags: #coding #interview

Topics: [[Interview Qs]]

