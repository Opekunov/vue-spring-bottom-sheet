<p align="center">
  <h1 align="center">Vue Spring Bottom Sheet</h1>
  <p align="center">Modern, performant bottom sheet component for Vue 3 with spring physics, morphing & gestures</p>
</p>

<p align="center">
  <a href="https://www.npmjs.com/package/@opekunov/vue-spring-bottom-sheet"><img src="https://img.shields.io/npm/v/@opekunov/vue-spring-bottom-sheet.svg" alt="npm version"></a>
  <a href="https://www.npmjs.com/package/@opekunov/vue-spring-bottom-sheet"><img src="https://img.shields.io/npm/dm/@opekunov/vue-spring-bottom-sheet.svg" alt="npm downloads"></a>
  <a href="https://github.com/opekunov/vue-spring-bottom-sheet/blob/master/LICENSE"><img src="https://img.shields.io/npm/l/@opekunov/vue-spring-bottom-sheet.svg" alt="license"></a>
</p>

<p align="center">
  <a href="#features">Features</a> &bull;
  <a href="#demo">Demo</a> &bull;
  <a href="#installation">Installation</a> &bull;
  <a href="#usage">Usage</a> &bull;
  <a href="#api">API</a> &bull;
  <a href="#styling">Styling</a>
</p>

> Based on [megaarmos/vue-spring-bottom-sheet](https://github.com/megaarmos/vue-spring-bottom-sheet)

**English** | **[Русский](./README.ru.md)**

---

## Features

- Spring physics animations (configurable mass, stiffness, damping)
- iOS-like morphing (compact -> expanded -> fullscreen)
- Drag gestures with velocity-based swipe detection
- Snap points (pixels or percentages)
- v-model support
- Keyboard accessibility (focus trap)
- Teleport support
- Body scroll locking
- Slots: header, content, footer
- CSS custom properties for theming
- TypeScript support
- Vue 3.3+

## Demo

**[Live Examples](https://opekunov.github.io/vue-spring-bottom-sheet/)**

## Installation

```bash
# npm
npm install @opekunov/vue-spring-bottom-sheet

# bun
bun add @opekunov/vue-spring-bottom-sheet

# yarn
yarn add @opekunov/vue-spring-bottom-sheet

# pnpm
pnpm add @opekunov/vue-spring-bottom-sheet
```

## Usage

### Basic

```vue
<script setup lang="ts">
import { useTemplateRef } from 'vue'
import BottomSheet from '@opekunov/vue-spring-bottom-sheet'
import '@opekunov/vue-spring-bottom-sheet/dist/style.css'

const sheet = useTemplateRef('sheet')
</script>

<template>
  <button @click="sheet?.open()">Open</button>

  <BottomSheet ref="sheet">
    Your content here
  </BottomSheet>
</template>
```

### With v-model

```vue
<script setup lang="ts">
import { ref } from 'vue'
import BottomSheet from '@opekunov/vue-spring-bottom-sheet'
import '@opekunov/vue-spring-bottom-sheet/dist/style.css'

const isOpen = ref(false)
</script>

<template>
  <button @click="isOpen = true">Open</button>
  <BottomSheet v-model="isOpen">Your content</BottomSheet>
</template>
```

### Snap Points

```vue
<BottomSheet ref="sheet" :snap-points="[200, '50%', '100%']" :initial-snap-point="0">
  Content with three snap positions
</BottomSheet>
```

### Spring Physics

```vue
<BottomSheet ref="sheet" :spring-config="{ mass: 1, stiffness: 300, damping: 30 }">
  Bouncy spring animation
</BottomSheet>
```

### Morphing (iOS-style)

Requires at least 2 snap points (ideally 3: compact / expanded / fullscreen).

```vue
<BottomSheet
  ref="sheet"
  :snap-points="['25%', '50%', '100%']"
  :morphing="{ compactHorizontalInset: 16, compactBottomInset: 16, compactCornerRadius: 20 }"
>
  Morphs from floating card to fullscreen
</BottomSheet>
```

### Slots

```vue
<BottomSheet ref="sheet">
  <template #header>Header</template>
  Main content
  <template #footer>Footer</template>
</BottomSheet>
```

### Nuxt

Wrap in `<ClientOnly>`:

```vue
<ClientOnly>
  <BottomSheet ref="sheet">Content</BottomSheet>
</ClientOnly>
```

## API

### Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `duration` | `number` | `250` | Animation duration (ms) |
| `snapPoints` | `Array<number \| '${number}%'>` | `[instinctHeight]` | Snap positions |
| `initialSnapPoint` | `number` | `0` | Initial snap point index |
| `blocking` | `boolean` | `true` | Block background interactions |
| `canSwipeClose` | `boolean` | `true` | Allow swipe-to-close |
| `swipeCloseThreshold` | `number \| string` | `'50%'` | Distance to trigger close |
| `canBackdropClose` | `boolean` | `true` | Close on backdrop tap |
| `expandOnContentDrag` | `boolean` | `true` | Expand by dragging content |
| `teleportTo` | `string \| RendererElement` | `'body'` | Teleport target |
| `teleportDefer` | `boolean` | `false` | Defer teleport (Vue 3.5+) |
| `forceMount` | `boolean` | `false` | Keep in DOM when closed |
| `headerClass` | `string` | `''` | Header CSS class |
| `contentClass` | `string` | `''` | Content CSS class |
| `footerClass` | `string` | `''` | Footer CSS class |
| `morphing` | `boolean \| MorphingConfig` | `false` | iOS-like morphing |
| `springConfig` | `SpringConfig` | `undefined` | Spring physics config |
| `modelValue` | `boolean` | `undefined` | v-model binding |

> Interactive elements (inputs, textareas, etc.) inside the sheet keep native touch behavior. Use `data-vsbs-no-drag` attribute to opt out of drag on custom elements.

### Methods

| Method | Description |
|---|---|
| `open()` | Open the sheet |
| `close()` | Close the sheet |
| `snapToPoint(index)` | Snap to a specific point |

### Events

| Event | Payload | Description |
|---|---|---|
| `opened` | - | Sheet finished opening |
| `opening-started` | - | Sheet started opening |
| `closed` | - | Sheet finished closing |
| `closing-started` | - | Sheet started closing |
| `dragging-up` | - | User is dragging up |
| `dragging-down` | - | User is dragging down |
| `snapped` | `index: number` | Snapped to a point |
| `instinctHeight` | `height: number` | Content height changed |

## Styling

Override via CSS custom properties:

```css
.my-sheet {
  --vsbs-backdrop-bg: rgba(0, 0, 0, 0.5);
  --vsbs-background: #fff;
  --vsbs-border-radius: 16px;
  --vsbs-max-width: 640px;
  --vsbs-padding-x: 16px;
  --vsbs-handle-background: rgba(0, 0, 0, 0.28);
  --vsbs-shadow-color: rgba(89, 89, 89, 0.2);
  --vsbs-border-color: rgba(46, 59, 66, 0.125);
  --vsbs-outer-border-color: transparent;
}
```

## License

[MIT](./LICENSE)
