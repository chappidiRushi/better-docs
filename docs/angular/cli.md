---
id: angular-cli
title: Angular CLI
sidebar_position: 3
---

# CLI

> Full reference of Angular 21 CLI commands, options, and usage.

---

## Overview

* **Angular CLI** scaffolds apps, components, services, modules, libraries, and more.
* Supports **standalone components**, **strict mode**, **tree-shakable providers**, and **workspace management**.
* Provides commands for **serve, build, test, lint, update, add, generate, deploy, and environment configuration**.

---

## Create a New Project

<details>
<summary>Example</summary>

```bash
ng new my-app --standalone --strict --routing --style=scss --package-manager=npm --skip-install
```

</details>

Options:

* `--standalone` → standalone components by default
* `--strict` → strict TypeScript, template, and Angular settings
* `--routing` → include router setup
* `--style` → css, scss, sass, less, styl
* `--package-manager` → npm / yarn / pnpm
* `--skip-install` → don’t install packages immediately
* `--minimal` → minimal setup
* `--inline-style`, `--inline-template` → embed in component
* `--skip-git` → skip Git initialization

---

## Serve the Application

<details>
<summary>Example</summary>

```bash
ng serve --open --port 4200 --configuration=development
```

</details>

Options:

* `--open` → open browser automatically
* `--port` → specify port number
* `--host` → bind to specific host
* `--ssl` → enable https
* `--proxy-config` → use proxy file
* `--live-reload` → enable/disable live reload
* `--configuration` → select config from angular.json

---

## Build the Application

<details>
<summary>Example</summary>

```bash
ng build --configuration=production --output-path=dist/my-app --watch
```

</details>

Options:

* `--output-path` → output directory
* `--configuration` → production / development / custom
* `--watch` → rebuild automatically on changes
* `--source-map` → include source maps
* `--vendor-chunk` → separate vendor bundle
* `--optimization` → enable build optimizations
* `--aot` → enable ahead-of-time compilation
* `--build-optimizer` → enable tree-shaking and optimizations

---

## Generate Artifacts

<details>
<summary>Example</summary>

```bash
ng generate component my-component --standalone --flat --skip-tests
ng generate service my-service --providedIn=root
ng generate module my-module --routing
ng generate class my-class
ng generate directive my-directive
ng generate pipe my-pipe
ng generate interface my-interface
ng generate enum my-enum
```

</details>

Options:

* `--flat` → no subfolder
* `--skip-tests` → don’t generate spec file
* `--standalone` → standalone component
* `--providedIn=root` → singleton service
* `--module` → specify module to declare in
* `--project` → specify project in workspace

---

## Run Tests

<details>
<summary>Example</summary>

```bash
ng test --watch --browsers=ChromeHeadless
ng e2e
```

</details>

Options:

* `--watch` → rerun on file changes
* `--browsers` → select browser
* `--code-coverage` → generate coverage report
* `--configuration` → use test-specific config

---

## Lint and Format

<details>
<summary>Example</summary>

```bash
ng lint --fix
ng format:write
```

</details>

Options:

* `--fix` → auto-fix lint issues
* `ng format:write` → formats code
* `ng format:check` → check formatting without writing

---

## Update Angular

<details>
<summary>Example</summary>

```bash
ng update @angular/core @angular/cli --next --force
```

</details>

Options:

* `--next` → update to next pre-release
* `--force` → override peer dependency checks
* `--all` → update all packages

---

## Add Packages

<details>
<summary>Example</summary>

```bash
ng add @angular/material --defaults
```

</details>

* Installs and configures third-party libraries
* `--defaults` → skip prompts
* Supports Angular Schematics for setup

---

## Environment & Configuration

<details>
<summary>Example</summary>

```bash
ng config projects.my-app.architect.build.options.outputPath dist/my-app
```

</details>

* Modify angular.json settings
* Adjust build paths, assets, scripts, styles, and optimization
* `--global` → change global config

---

## Deployment

<details>
<summary>Example</summary>

```bash
ng deploy --base-href=/my-app/ --configuration=production
```

</details>

* Supports deployers like Firebase, Netlify, GitHub Pages
* `--base-href` → base URL
* `--configuration` → select build configuration

---

## Other Useful Commands

<details>
<summary>Example</summary>

```bash
ng doc ComponentName           # Open Angular docs for a class
ng version                      # Show Angular & CLI versions
ng analytics enable             # Enable CLI analytics
ng analytics disable            # Disable CLI analytics
ng xi18n                         # Extract i18n messages
```

</details>

This cheatsheet covers **all major Angular 21 CLI commands and options** with collapsible examples for Docusaurus.
