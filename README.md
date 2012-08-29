# Pointer Events
> A unified event system for the web platform

## Why Pointer Events?

Mouse events and Touch events are fundamentally different beasts in browsers today, and that makes it hard to write cross-platform apps.

For example, a simple finger paint app needs plenty of work to behave correctly with mouse and touch:

To draw in a mouse environment, the app needs to listen for mousedown, mousemove (only when a mouse button is pressed), and mouseup, but in a touch environment those events are touchstart, touchmove, and touchend.
Touch environments can emulate mouse events, but with many caveats:
- Mouse events are only fired after the touch sequence ends.
- Mouse events are not fired on elements without a click event handler. One must be attached by default, or directly on the element with “onclick”.
- Click events are not fired if the content of the page changes in a mousemove or mouseover event.
- Click events are fired 300ms after the touch sequence ends.
- More information: [Apple Developer Documentation](http://developer.apple.com/library/safari/#documentation/appleapplications/reference/safariwebcontent/HandlingEvents/HandlingEvents.htmls).
- Touch events are sent only to the element that received the touchstart. This is fundamentally different than mouse events, which fire on any element that is under the mouse. To make them behave similarly, touch events need to be retargeted with document.elementFromPoint.

This leads to applications needing to listen to 2 sets of events, mouse on desktop and touch for mobile. This is cumbersome and hard to maintain.

Instead, there should exist a set of events that are normalized such that they behave exactly the same, no matter the source: touch, mouse, stylus, skull implant, etc.
To do this right, this normalized event system needs to be available for all the web platform to use.

*Thus, PointerEvents!*

## Why not other event systems?

There are existing implementations of event unification, but each have caveats that we think make them not suitable:
- [pointer.js](https://github.com/borismus/pointer.js) nicely abstracts mouse and touch events, but does not solve the touch event targeting issue, and wraps addEventListener
- [Hammer.js](http://eightmedia.github.com/hammer.js/) requires wrapping all elements in a handler and does not dispatch DOM events
- [Sencha’s solution](http://www.sencha.com/products/touch) requires the whole Sencha Touch framework
- [Microsoft MSPointer Events](http://msdn.microsoft.com/en-us/library/ie/hh673557.aspx) solve the problem the best, but aren't standardized.

Therefore, we felt the need to create PointerEvents.

## What events are there?

- Basic screen movement
  - pointermove: a pointer moves, similar to touchmove or mousemove.
  - pointerdown: a pointer is activated, or a device button held.
  - pointerup: a pointer is deactivated, or a device button released.
  - pointerenter: a pointer crosses into an element’s bounding box.
  - pointerleave: a pointer leaves an element’s bounding box.
  - pointertap: an up/down pair is seen in rapid succession.
- Hold Events * not yet implemented *
  - pointerhold: a pointer is held down for 200ms.
  - pointerholdpulse: a pointer is *still* held down, pulses every 200ms.
  - pointerrelease: a pointer is released after holding.
- Drag Events * not yet implemented *
  - pointerdragstart: a pointer starts dragging across the screen.
  - pointerdrag: a pointer is being dragged across the screen, updates with position, fired on the target of the dragstart.
  - pointerdragend: a pointer stops dragging, fired on the target of the dragstart.
  - pointerdrop: a pointer stops dragging, fired on the last element the pointer entered.
  - pointerdragenter: a pointer enters an element's bounding box while dragging.
  - pointerdragleave: a pointer leaves an element’s bounding box while dragging.
  - pointerflick: a pointer moves rapidly across the screen, provides inertial information.
- Gesture Events * not yet implemented *
  - pointerscale: two or more pointers are spreading apart
  - pointerrotate: two or more pointers are rotating around a center point

You can also add your own events!
