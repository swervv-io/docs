# Swervv Documentation

[Installation](#installation)

[Configuration](#configuration)

[Usage](#usage)

[Examples](#examples)

[Typescript](#typescript)

<br>

## Installation

Simply add the Swervv script to your VDP page in either the `<head>` or `<body>`:

```html
<script async type="application/javascript" src="https://connect.swervv.io/loader.js" data-organization-id="ORGANIZATION_ID"></script>
```

<br>

> **Note** - Remember to replace `ORGANIZATION_ID` with the unique ID provided by Swervv during implementation. Contact [support\@swervv.io](mailto:support@swervv.io) for support.

<br>

> **Tip** - For advanced use cases, the Swervv script can also be [loaded programmatically](https://www.educative.io/answers/how-to-dynamically-load-a-js-file-in-javascript). Just remember to use [`setAttribute`](https://developer.mozilla.org/en-US/docs/Web/API/Element/setAttribute) for each parameter in your [configuration](#configuration).

<br>

## Configuration

Use [`data-* attributes`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/data-*) directly on the Swervv `<script>` for configuration. The following table lists available options: 

| Name | Required? | Default | Description |
| --- | --- | --- | --- |
| `data-organization-id` | Required | | Unique ID provided by Swervv during implementation. Contact [support\@swervv.io](mailto:support@swervv.io) for support. |
| `data-theme` | Optional | `#1F1F1F` | Optionally configure the theme color for the widget using a HEX color code. |
| `data-namespace` | Optional | `swervv` | The namespace for the Swervv property added to the `window` object. By default, the Swervv object is accessed via `window.swervv`. This option should only be used to avoid the unlikely event of an existing namespace collision. |

<br>

> **Note** - If using `data-namespace`, any further reference to `window.swervv` in this documentation should be `window.YOUR_CUSTOM_NAMESPACE` instead. Or, simply use `window[window.SwervvNamespace]`.

<br>

## Usage

With the Swervv script installed and configured, the Swervv object is now globally accessible via `window.swervv` or `window[window.SwervvNamespace]`.

Use the following methods according to your use case:


#### 1. Swervv button

Simply pass a container element and a VIN number to the `createButton` method and Swervv will create a button and manage everything else: 

```typescript
createButton(el: HTMLElement, vin: string)
```

<br>

> **Note** - When using `createButton`, the `.Swervv__Button` class can be used for stylistic adjustments. For example:
```html
<style type="text/css">
  .Swervv__Button {
    margin: 32px auto 0px auto;
    max-width: 670px;
  }
</style>
```

<br>

#### 2. Existing button

Pass an existing button element and a VIN number to the `registerButton` method and Swervv will manage the necessary listeners:

```typescript
registerButton(el: HTMLElement, vin: string)
```

<br>

> **Tip** - A previously registered button can be unregistered using the `unregisterButton` method. This simply removes the event listener added by Swervv and is typically only used in single page applications to prevent memory leaks:

```typescript
unregisterButton(el: HTMLElement)
```

<br>

#### 3. Manual control

Programmatically control the Swervv widget using the following methods in any of your event listeners:

```typescript
open(vin: string)
```

```typescript
close()
```

<br>

## Examples

#### 1. Usage with a static webpage (Create or Register Button):
```html
<!-- example_vdp.html -->

<body>

  ...

  <!-- Install Script -->
  <script async type="application/javascript" src="https://connect.swervv.io/loader.js" data-organization-id="ORGANIZATION_ID"></script>

  <!-- Option 1: Create Button -->
  <script type="text/javascript">
    var containerEl = document.getElementById('example-container');
    window.swervv.createButton(containerEl, '{{vin}}');
  </script>

  <!-- Option 2: Register Existing Button -->
  <script type="text/javascript">
    var existingButtonEl = document.getElementById('example-button');
    window.swervv.registerButton(existingButtonEl, '{{vin}}');
  </script>

</body>
```

<br>

#### 2. Usage with a SPA (single page application):
```html
<!-- example_index.html -->

<head>

  ...

  <!-- Could also be loaded programmatically depending on use case -->
  <script async type="application/javascript" src="https://connect.swervv.io/loader.js" data-organization-id="ORGANIZATION_ID"></script>

</head>
```

```html
<!-- example_component.[jsx, vue, etc] -->

<ExampleButton
  @click="exampleHandler"
>
  Schedule Test Drive
</ExampleButton>
```
```typescript
/** example_component.[jsx, vue, etc] **/

function exampleHandler() {
  window.swervv.open(props.vehicle.vin) // <- Dynamic vin passed via props
}
```

<br>

## Typescript

If using with Typescript, the following type definition can be used to extend the `window` object by providing typings for the Swervv API:

```typescript
declare global {
  interface Window {
    SwervvNamespace: string
    swervv: { // <- namespace passed via configuration
      open: (vin: string) => void
      close: () => void
      createButton: (el: HTMLElement, vin: string) => void
      registerButton: (el: HTMLButtonElement, vin: string) => void
      unregisterButton: (el: HTMLButtonElement, vin: string) => void
    }
  }
}
```
