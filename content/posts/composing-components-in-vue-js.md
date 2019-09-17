---
title: Composing Components in Vue.js
date: 2019-09-11
tags: ['Vue.js', 'Tips']
canonical_url: false
description: "Component Composition can be understood in two ways. Usually people think about composing a component using mixins (and soon composition functions thanks to the upcoming Composition API). However, what I would like to talk about is composition where we connect several components together to form a new one that combines the functionalities of the smaller components. But let’s start from the beginning – the why."

---

_Component Composition_ can be understood in two ways. Usually people think about composing **a component** using mixins (and soon composition functions thanks to the upcoming Composition API). However, what I would like to talk about is composition where we connect several components together to form a new one that combines the functionalities of the smaller components. But let’s start from the beginning – the **why**.

Components are often most useful when they are reusable. This is usually accomplished by creating props that alter the behaviour of the component. An example would be a button component that can accept a `text` prop that will then be displayed inside the button.

```html
<!-- BaseButton.vue -->
<template>
  <button class="nice-button" type="button">
    {{ text }}
  </button>
</template>

<script>
export default {
  props: ['text']
}
</script>
```

As our applications grow, we will require more configuration possibilities, resulting in more props. Let’s take a look at another example where our button has to also display an icon on either the left side of the text or the right side. Let’s assume we have an `BaseIcon` component that makes it possible to display an icon. The code would then look like this:

```html
<template>
  <button class="nice-button" type="button">
    <BaseIcon v-if="leftIcon" :icon="leftIcon"/>
    {{ text }}
    <BaseIcon v-if="rightIcon" :icon="rightIcon"/>
  </button>
</template>

<script>
export default {
  props: ['text', 'leftIcon', 'rightIcon']
}
</script>
```

As you can see, our template grew a bit bigger, now having two conditionals `v-if` that will be responsible for displaying the icons. Our props list also increased by two. Now imagine we add more requirements for the button. For example, if we wanted to make it so that you could also replace the whole content of the button with a spinner, in case it’s loading. This would additionally complicate the template with yet another conditional.

```html
<template>
  <button class="nice-button" type="button">
    <BaseSpinner v-if="isLoading"/>
    <template v-else>
      <BaseIcon v-if="leftIcon" :icon="leftIcon"/>
      {{ text }}
      <BaseIcon v-if="rightIcon" :icon="rightIcon"/>
    </template>
  </button>
</template>
```

A component that was supposed to be just a Button, has grown pretty quickly and as you probably noticed – at this point, any new requirement will make it even more complicated, introducing more template and props. And the requirements hardly ever end here. This in turn might lead to a hard time maintaining it and testing, because instead of separating concerns, we let the component soak more features, more responsibilities.

## Alternative solution

That’s why at this point it is recommended to take a step back and start thinking about slots. Please consider this example.

```html
<template>
  <button class="nice-button" type="button">
    <slot/>
  </button>
</template>
```

It already covers all the requirements that the previous component did and tens of other possible needs. Take a look:

```html
<BaseButton>
  Submit
  <BaseIcon icon="arrow-right"/>
</BaseButton>
```

As you can see, the parent has complete control over what is displayed inside the button component. You can even pass other components inside it. Just like in a standard, HTML button element.

One might argue that now we’re simply moving the responsibilities from the button to the parent component, forcing ourselves to repeat the slot content over and over again. And this is true. And good. That’s because, such a button component becomes a perfect base for composition. You can now create more specialised components that can include icons, loading indicators and more. Here’s an example of a `FormSubmitButton` component.

```html
<template>
  <BaseButton @click="sendForm">
    <BaseSpinner v-if="isLoading"/>
    <template v-else>
      Submit
      <BaseIcon icon="arrow-right"/>
    </template>
  </BaseButton>
</template>
```

To sum things up, whenever you are concerned about distributing content, you should think about using slots. But what does content distribution mean, you ask? I’d say, if a component is supposed to act as layout or wrapper (think how you use a `<div>` or `<form>`) for your content, it’s probably good to use slots for distributing that content. Otherwise, props are the way to go.

## Advanced use of slots and scoped slots

Now that we understand the importance of component composition let’s talk about more advanced techniques. For those, we will use scoped slots. You can read more about how they work [here](https://vuejs.org/v2/guide/components-slots.html#Scoped-Slots). Scoped slots are most commonly used to let the parent component decide how data coming from the child component should be displayed. Or in other words – it allows a child component to expose some of its data inside the slots that it has. There is, however, much more that you can do using scoped slots.

First of all, besides data/state, you can also pass methods to the scoped slot. This allows us to give more control over the child component to the parent, which in turn opens up lots of new possibilities.

Let’s take a look at this `BaseTooltip` component.

```html
<template>
  <span class="tooltip">
    <span
      @mouseenter="setIsOpen(true)"
      @mouseleave="setIsOpen(false)"
      class="tooltip-trigger"
    >
      <!-- the default slot -->
      <slot/>
    </span>
    <div v-show="isOpen" class="tooltip-content">
      <slot name="content"/>
    </div>
  </span>
</template>

<script>
export default {
  data () {
    return {
      isOpen: false
    }
  },
  methods: {
    setIsOpen (isOpen) {
      this.isOpen = isOpen
    }
  }
}
</script>
```

It can now be used like this:

```html
How you
<BaseTooltip>
  doing?
  <div v-slot:content>
    It’s Joey!
  </div>
</BaseTooltip>
```

The component works as follows: once your mouse hovers over the `'doing?'` string, it will toggle the tooltip content thanks to the `mouseenter` and `mouseleave` event listeners that trigger the `setIsOpen` method that in turn changes the `isOpen` value.

This implementation is probably fine to be used as a tooltip. It can be easily be extended to include a delay before it opens or before it closes.

However, what if we would like to build something else on top of it? What if we would like to create a dropdown component?

A dropdown is a button that once clicked, shows some extra content underneath, pretty similar to how the tooltip does it. But, we don’t want it to happen on hover. For that we need to make a few modifications to our `BaseTooltip` code and make use of scoped slots. And this is where the magic of Vue’s scoped slots comes into use.

```html
<template>
  <span class="tooltip">
    <!-- Creating a "trigger" slot, where we expose
         the "setIsOpen" method and the "isOpen" state -->
    <slot name="trigger" v-bind="{ setIsOpen, isOpen }">
      <span
        @mouseenter="setIsOpen(true)"
        @mouseleave="setIsOpen(false)"
        class="tooltip-trigger"
      >
        <slot/>
      </span>
    </slot>
    <div v-show="isOpen" class="tooltip-content">
      <slot name="content"/>
    </div>
  </span>
</template>
```

Now that the slot receives the method `setIsOpen` that can control the BaseTooltip’s `isOpen` state, we can make use of it creating the `BaseDropdown` component.

```html
<!-- BaseDropdown.vue -->
<template>
  <BaseTooltip>
    <!-- Notice that we use the "trigger" slot here
         instead of the default one-->
    <template v-slot:trigger=“{ setIsOpen, isOpen }”>
      <BaseButton @click.native=“setIsOpen(!isOpen)”>
        <slot/>
      </BaseButton>
    </template>
    <div v-slot:content>
      <slot name=“content” v-bind=“{ setIsOpen }”/>
    </div>
  </BaseTooltip>
</template>
```

What happens now is that when we click on the `BaseButton` we trigger the exposed `setIsOpen` method, which in turn changes the `isOpen` state of the `BaseTooltip`. We could also trigger it on focus/blur or wait for the user to press a key while focusing on the button, to open it. We got complete control over the tooltip.

Lets add one last touch – the [vue-global-events](https://github.com/shentao/vue-global-events) library, that will handle the closing of the dropdown when the user clicks outside of the dropdown.

```html
<!-- BaseDropdown.vue -->
<template>
  <BaseTooltip>
    <template v-slot:trigger=“{ setIsOpen, isOpen }”>
      <BaseButton @click.native=“setIsOpen(!isOpen)”>
        <!-- When `isOpen` is `true`, we activate the global `click` event listener, that triggers the `handleOutsideClick` method -->
        <GlobalEvents v-if=“isOpen” @click=“handleOutsideClick($event, setIsOpen)”/>
        <slot/>
      </BaseButton>
    </template>
    <div v-slot:content>
      <slot name=“content” v-bind=“{ setIsOpen }”/>
    </div>
  </BaseTooltip>
</template>

<script>
import BaseButton from './BaseButton'
import BaseTooltip from './BaseTooltip'
import GlobalEvents from 'vue-global-events'

export default {
  components: {
    BaseTooltip,
    BaseButton,
    GlobalEvents
  },
  methods: {
    handleOutsideClick (e, cb) {
      // Here we receive the click event and check if the click target
      // is within the dropdown itself.
      // If it’s not, we trigger the callback,
      // that in this case is the `setIsOpen` method
      if (!this.$el.contains(e.target)) cb(false)
    }
  }
}
</script>
```

And that’s it! We have a fully functional dropdown component that we can easily reuse and use to compose entirely new components. For example, we could build a [select component](https://github.com/shentao/composing-components/blob/master/src/components/AppSelect.vue) on top of it. Or replace the `AppButton` with an `input` and make it into a searchable autocomplete component.

<iframe src="https://codesandbox.io/embed/composing-components-blog-0sjpp?fontsize=14" title="composing-components-blog" allow="geolocation; microphone; camera; midi; vr; accelerometer; gyroscope; payment; ambient-light-sensor; encrypted-media; usb" style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;" sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"></iframe>

## Wrap-up

If this doesn’t sound overly exciting to you, that’s understandable – it was a rather basic example after all. However, I do believe that it shows some of the incredible potential that slots and especially scoped slots have. It also enables some really useful design patterns that we will explore in the upcoming articles.

As a side-note though — I wouldn’t exactly recommend building everything this way. As powerful and fancy as it might seem, it can be completely unnecessary for solving simple problems, where going with props is just good enough. The important lesson here is to not get stuck in that approach where we only rely on endless configuration options (props), rather than being able to compose the required functionality out of reusable components. If you find yourself in such a that trap, take a step back and refactor.

Keep in mind, that when you aim for building reusable components this way, their responsibilities are usually much more limited and thus their API contract is much cleaner. This makes it easier to test and reason about. It also forces us to think more about the architecture of things. Which is always a good thing.

### tl;dr;

> _More slots, less props._
