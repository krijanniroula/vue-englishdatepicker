# vue-datepicker

> An easy-to-use and customizable date picker component powered by Vue js

# Demo:s

https://codesandbox.io/s/eager-rubin-5zcm9

## Install

```shell
npm install vue-englishdatepicker
```

## Quick Start

```javascript

import VueEnglishdatepicker from 'vue-englishdatepicker';
 
export default {
  components: {
    VueEnglishdatepicker,
  },
  // rest of the component
}
 
Or even used via <script> tag in the browser directly:
 
<script src="https://unpkg.com/vue"></script>
<script src="https://unpkg.com/vue-englishdatepicker"></script>
...
<vue-englishdatepicker />
...


```

## Examples

```vue
<template>
  <vue-englishdatepicker />
</template>
```

## Customizable Properties

The following customizable properties can be added to the component

1. classValue
2. calenderType
3. placeholder
4. format
5. value
6. yearSelect
7. monthSelect

## Examples - classValue

This works exactly as class properties. Eg: classValue="form-control" (boostrap class)

```vue
<template>
  <vue-englishdatepicker classValue="datepicker" />
</template>
<style>
.datepicker {
  width: 50px;
  height: 20px;
}
</style>
```

## Examples - placeholder

```vue
<template>
  <vue-englishdatepicker placeholder="YYYY-MM-DD" />
</template>
```

## Examples - format

It uses moment js API. Read the moment js documentation for the format. Same format style will be applied to the datepicker.

<p align="center">
  <a href="https://momentjs.com/docs/#/displaying/format/">
  <h2>Moment Js format Docs</h2>
  </a>
</p>

```vue
<template>
  <vue-englishdatepicker format="YYYY-MM-DD" />
</template>
```

## Examples - value

Initial value for the datepicker.

```vue
<template>
  <vue-englishdatepicker value="2020-10-10" />
</template>
```

## Examples - yearSelect

The dropdown year select can be turned off using boolean type to yearSelect

```vue
<template>
  <vue-englishdatepicker :yearSelect="false" />
</template>
```

## Examples - monthSelect

The dropdown month select can be turned off using boolean type to monthSelect

```vue
<template>
  <vue-englishdatepicker :monthSelect="false" />
</template>
```

## Examples - All in one

```vue
<template>
  <vue-englishdatepicker
    placeholder="YYYY-MM-DD"
    format="YYYY-MM-DD"
    value="2020-10-10"
    :yearSelect="false"
    :monthSelect="false"
  />
</template>
```
