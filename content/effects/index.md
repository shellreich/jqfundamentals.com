title: Animating Your Pages with jQuery
previous:
  name: Events & Event Delegation
  path: /chapter/events
next:
  name: AJAX & Deferreds
  path: /chapter/ajax-deferreds
---

jQuery makes it trivial to add simple effects to your page. Effects can use the built-in settings, or provide a customized duration. You can also create custom animations of arbitrary CSS properties.

See the [effects documentation](http://api.jquery.com/category/effects/) for
complete details on jQuery effects.

**An important note about animations:** In modern browsers, and especially on
mobile devices, it is often much more efficient to achieve animations using
CSS, rather than JavaScript. The details of doing this are beyond the scope of
this guide, but if you are only targeting mobile, or browsers that support CSS
animations, then you should not use jQuery for animations. You may also want to
consider setting `jQuery.fx.off` to true on low-resource devices; doing so will
cause animation methods to immediately set elements to the desired state,
rather than animating to that state.

## Built-in Effects

Frequently used effects are built into jQuery as methods that you can call on
any jQuery object:

- **`.show()`** Show the selected elements.
- **`.hide()`** Hide the selected elements.
- **`.fadeIn()`** Animate the opacity of the selected elements to 100%.
- **`.fadeOut()`** Animate the opacity of the selected elements to 0%.
- **`.slideDown()`** Display the selected elements with a vertical sliding motion.
- **`.slideUp()`** Hide the selected elements with a vertical sliding motion.
- **`.slideToggle()`** Show or hide the selected elements with a vertical sliding motion, depending on whether the elements are currently visible.

Once you've made a selection, it's simple to apply an effect to that selection.

    $('.hidden').show();

You can also specify the duration of built-in effects. There are two ways to do
this: you can specify a time in milliseconds ...

    $('.hidden').show(300);

... or use one of the pre-defined speeds:

    $('.hidden').show('slow');

The predefined speeds are specified in the `jQuery.fx.speeds` object; you can
modify this object to override the defaults, or extend it with new names:

    jQuery.fx.speeds.blazing = 50;
    jQuery.fx.speeds.turtle = 3000;

Often, you'll want to do something once an animation is done — if you try to do
it before the animation completes, it may affect the quality of the animation,
or it may remove elments that are part of the animation. You can provide a
callback function to the animation methods if you want to specify what should
happen when the effect is complete. Inside the callback, `this` refers to the
raw DOM element that the effect was applied to. Just like with event callbacks,
we can turn it into a jQuery object by passing it to the `$()` function: `$(this)`.

    $('p.old').fadeOut(300, function() {
      $(this).remove();
    });

Note that if your selection doesn't contain any elements, then your callback
will never run! If you need your callback to run regardless of whether there
are elements in your selection, you can create a [named function
expression](http://kangax.github.com/nfe/) and use it for both cases:

    var thingToRemove = $('p.old');
    var thingToAnimate = $('#nonexistent');
    var cb = function() {
      thingToRemove.remove();
    };

    if (thingToAnimate.length) {
      thingToAnimate.show('slow', cb);
    } else {
      cb();
    }

## Custom effects with .animate()

If the built-in animations don't suit your needs, you can use `.animate()` to
create custom animations of any CSS property (with the notable exception of
animating color, for which there is a [plugin](https://github.com/jquery/jquery-color/)).

The `.animate()` method takes up to three arguments:

- an object defining the properties to be animated
- the duration of the animation, in milliseconds
- a callback function

The `.animate()` method can animate to a specified final value, or it can
increment an existing value.

    $('.funtimes').animate(
      {
        left : '+=50', // increase by 50
        opacity : 0.25,
        fontSize : '12px'
      },
      300,
      function() {
        // callback to execute when the animation is done
      }
    );

Note that if you want to animate a CSS property whose name includes a hyphen,
you will need to use a "[camel cased](http://en.wikipedia.org/wiki/CamelCase)"
version of the property name. For example, the `font-size` property must be
referred to as `fontSize`.

## Managing animations

jQuery provides two important methods for managing animations.

- `.stop()` will stop currently running animations on the selected elements.
- `.delay()` will pause before the execution of the next animation method. Pass
  it the number of milliseconds you want to wait.

jQuery also provides methods for managing the effects queue, creating custom
queues, and adding custom functions to these queues. These methods are beyond
the scope of this guide, but [you can read about them
here](http://api.jquery.com/category/effects/).
