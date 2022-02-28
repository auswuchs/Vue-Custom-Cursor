# Custom cursor in Vue 3 with GSAP


Change the mouse cursor in Vue 3 with custom typescript cursor component.

## Necessary dependencies

First of all install GSAP via npm:

```
npm i gsap
```

## Custom cursor component
Code of file CustomCursor.vue:

```
<template>
  <div class="custom-cursor">
    <div class="cursor-big custom-cursor__ball custom-cursor__ball--big "></div>
  </div>
</template>

<script setup lang="ts">
import gsap from "gsap";
import { onMounted } from 'vue'

const props = withDefaults(defineProps<{ hoverClass?: string, cursorColor?: string }>(), {
  hoverClass: 'cursor-hover',
  cursorColor: 'rgba(255, 255, 255, 0.22)'
})


onMounted(() => {
  const cursorBig = document.querySelector('.cursor-big'),
    links = document.getElementsByTagName("a"),
    withClassHover = document.getElementsByClassName(props.hoverClass),
    withHover = [...links, ...withClassHover];

  cursorBig.style.backgroundColor = props.cursorColor


  document.addEventListener("mousemove", onMouseMove);
  document.addEventListener("mousedown", onMouseHover);
  document.addEventListener("mouseup", onMouseHoverOut);
  document.addEventListener("mouseenter", () => {
    cursorBig.style.backgroundColor = props.cursorColor
    cursorBig.style.opacity = 1;
  });
  document.addEventListener("mouseleave", () => {
    cursorBig.style.opacity = 0;
  });
  withHover.forEach((element) => {
    element.addEventListener("mouseover", onMouseHover);
    element.addEventListener("mouseout", onMouseHoverOut);
  })

  // Event Handlers
  function onMouseMove (e) {
    gsap.to(cursorBig, 0.4, {
      x: e.clientX - 18,
      y: e.clientY - 18
    });
  }
  function onMouseHover () {
    gsap.to(cursorBig, 0.3, {
      scale: 3
    });
  }
  function onMouseHoverOut () {
    gsap.to(cursorBig, 0.3, {
      scale: 1
    });
  }
})

</script>




<style>
  @media screen and (min-width: 1100px) {
    .custom-cursor__ball {
      position: fixed;
      top: 0;
      left: 0;
      mix-blend-mode: normal;
      z-index: 99999;
      opacity: 0;
      pointer-events: none;
      transition: opacity 0.5s ease;
    }

    .custom-cursor__ball--big {
      content: '';
      width: 25px;
      height: 25px;
      border-radius: 50%;
    }
  }
</style>
```

## Import to App.vue


```
<template>
  <Custom-Cursor />
</template>
```

## Pass some optional props
#### **`hoverClass`** defines the elements on what the cursor should respond to.

```
<template>
  <Custom-Cursor hoverClass="on-hover"/>
</template>
```
> Default `hoverClass` set to `"cursor-hover"`


#### **`cursorColor`** defines cursor color.
```
<template>
  <Custom-Cursor cursorColor="#FFFFFF"/>
</template>
```

<template>
  <Custom-Cursor cursorColor="rgb(255, 255, 255)"/>
</template>
```

> Default `cursorColor` set to `"rgba(255, 255, 255, 0.22)"`

