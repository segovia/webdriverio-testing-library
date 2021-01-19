<div align="center">
<h1>webdriverio-testing-library</h1>

</a>

<p>WebdriverIO utilities that encourage good testing practices laid down by dom-testing-library.</p>

<p><strong>Based heavily on the great work on <a href="https://github.com/testing-library/nightwatch-testing-library">nightwatch-testing-library</a></strong></p>

</div>

<hr />

[![Build Status][build-badge]][build]
[![version][version-badge]][package]
[![MIT License][license-badge]][license]
[![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg)](https://github.com/semantic-release/semantic-release)

[![PRs Welcome][prs-badge]][prs]
[![Code of Conduct][coc-badge]][coc]

<div align="center">
<a href="https://testingjavascript.com">
<img width="500" alt="TestingJavaScript.com Learn the smart, efficient way to test any JavaScript application." src="https://raw.githubusercontent.com/kentcdodds/cypress-testing-library/master/other/testingjavascript.jpg" />
</a>
</div>

## The problem

You want to use [dom-testing-library](https://github.com/kentcdodds/dom-testing-library) methods in your [webdriverio][webdriverio] tests.

## This solution

Based heavily on [nightwatch-testing-library][nightwatch-testing-library]

This allows you to use all the useful [dom-testing-library](https://github.com/kentcdodds/dom-testing-library) methods in your tests.

## Table of Contents

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Installation](#installation)
- [Usage](#usage)
- [Other Solutions](#other-solutions)
- [LICENSE](#license)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Installation

This module is distributed via [npm][npm] which is bundled with [node][node] and
should be installed as one of your project's `devDependencies`:

```
npm install --save-dev webdriverio-testing-library
```

## Usage

### setupBrowser

Accepts a WebdriverIO BrowserObject and resolves with
dom-testing-library queries modifed to return WebdriverIO Elements. All the
queries are async, including queryBy and getBy variants, and are bound to
`document.body` by default.

```
const {setupBrowser} = require('webdriverio-testing-library');

it('can click button', async () => {
  const {getByText} = await setupBrowser(browser)

  const button = await getByText('Button Text');
  await button.click();

  expect(await button.getText()).toEqual('Button Clicked')
})
```

Queries are also added to the BrowserObject and Elements as commands. The
browser commands are scoped to the `document.body` as above and the Element
commands are scoped to the element.

```
it('adds queries as browser commands', async () => {
  await setupBrowser(browser);

  expect(await browser.getByText('Page Heading')).toBeDefined()
})

it('adds queries as element commands scoped to element', async () => {
  await setupBrowser(browser);

  const nested = await browser.$('*[data-testid="nested"]');
  const button = await nested.getByText('Button Text')
  await button.click()

  expect(await button.getText()).toEqual('Button Clicked')
})
```

### within

Returns queries scoped to a WebdriverIO element

```
const {within} = require('webdriverio-testing-library')

it('within scopes queries to element', async () => {
  const nested = await browser.$('*[data-testid="nested"]');

  const button = await within(nested).getByText('Button Text');
  await button.click();

  expect(await button.getText()).toEqual('Button Clicked')
});
```

### configure

Lets you pass a config to dom-testing-library

```
const {configure} = require('webdriverio-testing-library')

beforeEach(() => {
  configure({testIdAttribute: 'data-automation-id'})
})
afterEach(() => {
  configure(null)
})

it('lets you configure queries', async () => {
  const {getByTestId} = await setupBrowser(browser)

  expect(await getByTestId('testid-in-data-automation-id-attr')).toBeDefined()
})
```

### Typescript

All the above methods are fully typed. To use the Browser and Element commands
added by `setupBrowser` the global `WebdriverIO` namespace will need to be
modified. Add the following to a typescript module:

```
import {WebdriverIOQueries} from 'webdriverio-testing-library';

declare global {
  namespace WebdriverIO {
    interface Browser extends WebdriverIOQueries {}
    interface Element extends WebdriverIOQueries {}
  }
}
```

If you are using the `@wdio/sync` framework you will need to use the
`WebdriverIOQueriesSync` type to extend the interfaces:

```
import {WebdriverIOQueriesSync} from 'webdriverio-testing-library';

declare global {
  namespace WebdriverIO {
    interface Browser extends WebdriverIOQueriesSync {}
    interface Element extends WebdriverIOQueriesSync {}
  }
}
```

## Other Solutions

I'm not aware of any, if you are please [make a pull request][prs] and add it
here!

## LICENSE

MIT

[npm]: https://www.npmjs.com/
[node]: https://nodejs.org
[build-badge]: https://github.com/olivierwilkinson/webdriverio-testing-library/workflows/webdriverio-testing-library/badge.svg
[build]: https://github.com/olivierwilkinson/webdriverio-testing-library/actions?query=branch%3Amaster+workflow%3Awebdriverio-testing-library
[version-badge]: https://img.shields.io/npm/v/olivierwilkinson/webdriverio-testing-library.svg?style=flat-square
[package]: https://www.npmjs.com/package/olivierwilkinson/webdriverio-testing-library
[downloads-badge]: https://img.shields.io/npm/dm/olivierwilkinson/webdriverio-testing-library.svg?style=flat-square
[npmtrends]: http://www.npmtrends.com/olivierwilkinson/webdriverio-testing-library
[license-badge]: https://img.shields.io/npm/l/olivierwilkinson/webdriverio-testing-library.svg?style=flat-square
[license]: https://github.com/olivierwilkinson/webdriverio-testing-library/blob/master/LICENSE
[prs-badge]: https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square
[prs]: http://makeapullrequest.com
[coc-badge]: https://img.shields.io/badge/code%20of-conduct-ff69b4.svg?style=flat-square
[coc]: https://github.com/olivierwilkinson/webdriverio-testing-library/blob/master/other/CODE_OF_CONDUCT.md
[dom-testing-library]: https://github.com/testing-library/dom-testing-library
[webdriverio]: https://webdriver.io/
[nightwatch-testing-library]: https://github.com/testing-library/nightwatch-testing-library
