<div align="center" markdown="1">

<img src="https://github.com/18alantom/strawberry/assets/29507195/9cb6a348-3b02-4de7-be62-ee85cf594871" alt="strawberry logo" width="720"/>

Zero-dependency, build-free framework for the artisanal web.

[Website](https://strawberry.quest) · [How it works](https://18alan.space/posts/how-hard-is-it-to-build-a-frontend-framework.html) · [Docs](https://github.com/18alantom/strawberry/tree/main/docs)

</div>

> **Warning**
>
> Strawberry is in a very experimental phase; pre-alpha. Everything stated below
> works, but I am still figuring out the quickest and cleanest ways of doing
> things.

---

> Seriously, another frontend framework?

Yes, but, Strawberry is not like the usual frontend-framework.

It doesn't have any dependencies. It doesn't need a build-step to run. It's
tiny, less than 3KB when gzipped. Yet it does a lot of the core things the big,
spangly frameworks can do.

```html
<!-- Define Components -->
<template name="plum-p">
  <p style="color: plum"><slot /></p>
</template>

<!-- Initialize Strawberry -->
<script>
  const data = sb.init();
</script>

<!-- Use Components -->
<plum-p sb-mark="message"> A plum colored p element! </plum-p>

<!-- Dynamically Update Components -->
<script>
  data.message = 'Hello, World!';
</script>
```

[Here's](https://strawberry.quest/#inventory-example) a live example from
the website.

---

<div align="center" markdown="1">

**Index**

[Installation](#installation) · [Features](#features) · [Examples](#examples) · [Development](#development)

[Docs](https://github.com/18alantom/strawberry/tree/main/docs) · [Roadmap](https://github.com/18alantom/strawberry/blob/main/ROADMAP.md)

</div>

## Installation

If you wanna try it out, then run this 👇 command to setup a simple _starter_ page.

```bash
curl -so- https://raw.githubusercontent.com/18alantom/strawberry/main/setup.sh | bash
```

Or if you wanna just use it straight away, copy this 👇 script tag in the head of your html file:

```html
<script src="https://unpkg.com/sberry@0.0.0-alpha.1/dist/sb.min.js"></script>
```

## Features

Here're are a few of its features:

1. **Reactivity**: change your data and the UI updates.
2. **Composability**: create and use components.
3. **Build-free**: doesn't require a build-step. Link or [copy the lib](https://unpkg.com/sberry@0.0.0-alpha.1/dist/sb.min.js) and you're ready to go.
4. **Zero Dependencies**: has no dependencies. Uses WebAPIs for everything.
5. **Tiny**: [source code](https://github.com/18alantom/strawberry/blob/main/index.ts) is under 1000 CLOC.
6. **No VDOM**: directly updates the DOM.

Strawberry is and will be developed with these two hard constraints:

1. Zero dependencies.
2. No build step required to run it.

Other than this, there is also a soft constraint of keeping the source code light.

---

## Examples

Here are a couple of simple examples of a few things that Strawberry can do
right now.

**1. Basic Reactivity**: `innerText` is updated when `data.message` when
is set.

```html
<p sb-mark="message">Placeholder</p>

<script>
  data.message = 'Hello, Strawberry!';
</script>
```

**2. Computed Values**: `innerText` is updated with the computed value
`data.countMessage` when `data.count` is updated.

```html
<p sb-mark="countMessage">Placeholder</p>

<script>
  data.count = 0;
  data.countMessage = () => `The count is: ${data.count}`;

  data.count += 1;
</script>
```

**3. Conditional Rendering**: `p` is rendered only when `data.sayHi` is `true`.

```html
<template sb-if="sayHi">
  <p>Hi!</p>
</template>

<script>
  data.sayHi = true;
</script>
```

**4. Looping**: `ul` is populated with `li` when `data.list` is set. `innerText`
of the `li` are set from the list items.

```html
<ul sb-mark="list" sb-child="li"></ul>

<script>
  data.list = ['Strawberry', 'Mulberry', 'Raspberry'];
</script>
```

**5. Templates**: On running `sb.register`, the `red-p` element is defined and can be used.

```html
<template name="red-p">
  <p style="color: red"><slot /></p>
</template>

<red-p>Hi!</red-p>
```

**5. External Templates**: Templates can be defined in external files. They are loaded and registered using `sb.load`.

```html
<script>
  sb.load('./templates.html');
</script>

<red-p>Hi!</red-p>
```

**6. Nested Templates**: Templates can be nested, named slots can be used to

```html
<!-- Blue H1 Template -->
<template name="blue-h1">
  <h1 style="color: blue"><slot></slot></h1>
</template>

<!-- Red P Template -->
<template name="red-p">
  <p style="color: red"><slot></slot></p>
</template>

<!-- Div Template using the above two -->
<template name="user-div">
  <div>
    <blue-h1>
      <slot name="name" />
    </blue-h1>
    <red-p>
      <slot name="age" />
    </red-p>
  </div>
</template>

<script>
  const data = sb.init();
</script>

<body>
  <user-div sb-mark="user"></user-div>
</body>

<script>
  data.user = { name: 'Lin', age: 36 };
</script>
```

---

## Development

The development of Strawberry does has a direction, but no deadlines as I work
on this usually during the weekends.

Here's a
[road map](https://github.com/18alantom/strawberry/blob/main/ROADMAP.md). This
isn't exactly a road map, but a list of problems and maybe their solutions that
need to be implemented to further Strawberry.

### Running Dev Mode

Strawberry has only two dev dependencies: `esbuild` and `typescript`. Other than
that the [Live Preview](https://marketplace.visualstudio.com/items?itemName=ms-vscode.live-server)
VSCode plugin is used to serve the HTML files for dev.

To run Strawberry in dev mode:

```bash
# Clone the repo
git clone https://github.com/18alantom/strawberry

# Install esbuild and typescript
cd strawberry
yarn install

# Run in Dev mode
yarn dev
```

You can now create an HTML file in the repository and add `script:src` to link
the generated `index.js` file. You can serve this using the Live Preview plugin.

### Tests

Right now tests just involve linking strawberry to an html file (`test.*.html`),
and calling the `test` function to check whether a value is as expected. Success
and Failures are printed in the console.

See [`test.html`](https://github.com/18alantom/strawberry/blob/main/tests/test.html) for an example.

---

## Douglas Crockford on the XML of today

These are excerpts from the CoRecursive podcast on [JSON vs XML](https://corecursive.com/json-vs-xml-douglas-crockford/).

> It’s probably the JavaScript frameworks. They have gotten so big and so weird.
> People seem to love them. I don’t understand why.

And on web APIs.

> ...the browsers have actually gotten pretty good. The web standards thing have
> finally worked, and the web API is stable pretty much. Some of it’s still pretty
> stupid, but it works and it’s reliable.

Read the transcript on [CoRecursive](https://corecursive.com/json-vs-xml-douglas-crockford/#javascript-frameworks).
