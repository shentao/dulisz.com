---
title: Composing Components in Vue.js
date: 2019-05-01
tags: ['Vue.js', 'Tips']
canonical_url: false
coverImage: ./images/composing-components.jpg
description: "Components are often most useful when they are reusable. This is often accomplished by creating a set of props that can alter the behaviour of the component. An example of that would be a button component that can accept a text prop that will then be displayed inside the button."

---

Components are often most useful when they are reusable. This is often accomplished by creating props that alter the behavior of the component. An example would be a button component that can accept a `text` prop that will then be displayed inside the button.

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

As our applications grow, we will require more configuration possibilities, resulting in more props. Let’s take a look at another example where our button has to also display an icon on either the left side of the text or the right side. Let’s assume we have an `AppIcon` component that makes it possible to display an icon. The code would then look like this:

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
    <AppSpinner v-if="isLoading"/>
    <template v-else>
      <AppIcon v-if="leftIcon" :icon="leftIcon"/>
      {{ text }}
      <AppIcon v-if="rightIcon" :icon="rightIcon"/>
    </template>
  </button>
</template>
```

A component that was supposed to be just a Button, has grown pretty quickly and as you probably noticed – any new requirement will make it even more complex, by introducing more template and props. This in turn leads to a hard time maintaining it and testing. And the requirements hardly ever end here.

That’s why at this point it is recommended to take a step back and start thinking about slots. Please consider this example.

```html
<template>
  <button class="nice-button" type="button">
    <slot/>
  </button>
</template>
```

It already covers all the requirements that the previous component did and also tens of other possible needs.  Consider this example:

```html
<BaseButton>
  Submit
  <AppIcon icon="arrow-right"/>
</BaseButton>
```

As you can see, the parent has complete control over what is displayed inside the button component. You can even pass other components inside it. Just like in a standard, HTML button element.

One might argue that now we’re simply moving the responsibilities from the button to the parent component, forcing ourselves to repeat the slot content over and over again. And this is true. And good. That’s because, such a button component becomes a perfect base for composition. You can now create more focused components that can include icons, loading indicators and more. Here’s an example of a FormSubmitButton component.

```html
<template>
  <BaseButton @click="sendForm">
    <BaseSpinner v-if="isLoading"/>
    <template v-else>
      Submit
      <AppIcon icon="arrow-right"/>
    </template>
  </BaseButton>
</template>
```

To sum things up, whenever you are concerned about distributing content, you should think about using slots. But what does content distribution mean, you ask? I usually imagine it like this. If a component is supposed to act as layout or wrapper (think how you use a <div> or <form>) for your content, it’s probably good to use slots for distributing that content. Otherwise, props are the way to go.

## Advanced use of slots and scoped slots

Now that we understand the importance of component composition let’s talk about more advanced techniques. For those, we will use scoped slots. You can learn about how they work here). Scoped slots are most commonly used to let the parent component decide how data coming from the child component should be displayed. There is, however, much more that you can do using scoped slots.
First of all, besides state, you can also pass methods to the scoped slot. This allows us to give more control over the child component to the parent, which in turn opens up lots of new possibilities.
Let’s take a look at this BaseTooltip component.

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

The component works as follows: once your mouse enters the `'d``oing?'` string, it will open the tooltip content thanks to the `mouseenter` and `mouseleave` event listeners that trigger the `setIsOpen` method that in turn changes the `isOpen` value.

This implementation is probably fine for being used as a tooltip. However, what if we would like to build something else on top of it? What if we would like to create a dropdown component?
A dropdown is a button that once clicked shows some extra content underneath, pretty similar to how the tooltip does it. But, we don’t want it to happen on hover only. For that we need to make a few modifications to our AppTooltip code and make use of scoped slots.

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
      <slot name="content" v-bind="{ setIsOpen, isOpen }"/>
    </div>
  </span>
</template>
```

Now that the slot receives the method `setIsOpen` that can control the BaseTooltip’s `isOpen` state, we can make use of it creating the AppDropdown component.


```html
<!-- BaseDropdown.vue -->
<template>
  <BaseTooltip>
    <template v-slot:trigger=“{ setIsOpen, isOpen }”>
      <AppButton @click.native=“setIsOpen(!isOpen)”>
        <GlobalEvents v-if=“isOpen” @click=“handleOutsideClick($event, setIsOpen)”/>
        <slot/>
      </AppButton>
    </template>
    <div v-slot:content=“{ setIsOpen, isOpen }”>
      <slot name=“content” v-bind=“{ setIsOpen }”/>
    </div>
  </BaseTooltip>
</template>
```


TODO:

- Summary of the above
- Additional patterns
    - Renderless component working as a “Provider”
    - “Composition over inheritance”-like pattern (SearchResultItem vs SearchResultItemFile)
