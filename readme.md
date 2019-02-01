# update-notifier [![Build Status](https://travis-ci.org/yeoman/update-notifier.svg?branch=master)](https://travis-ci.org/yeoman/update-notifier)

> Update notifications for your CLI app. Supports git repos without a npm repository.

![](screenshot.png)

Inform users of your package of updates in a non-intrusive way.


**This is a fork which implements support for pretty much every type of repository that supports tags/versions (e.g. npm, GitHub, GitLab).\
This readme only shows parts of [update-notifier][un] documentation.
The package has all the functionality that [update-notifier][un] has as of Feburary 1, 2019 so please also consult the original docs for Information**

## notes for doc
man kann mit { repoChecks:{repo:repoLatest}} auch eigene checks übergeben. oder die vorhandenen (npm und gitlab) überschreiben.


#### Contents

- [Install](#install)
- [Usage](#usage)
- [API](#api)
- [Package-specific options](#_OPTIONS SPECIFIC TO THIS PACKAGE_)
- [About](#about)
- [Users](#users)


## Install

```
$ npm install update-notifier-repos
```


## Usage

### Simple, Comprehensive, Standard Options and Custom Message
see [update-notifier][un]

### New Options

```js
const notifier = updateNotifier({
	pkg,
	repo: {
	    type: 'npm | gitlab | github | <type declared in repoChecks>', // optional; will default to npm
	    url: 'https://gitlab.example.com', // optional; without /api/v4; will default for npm, GitLab and GitHub
	    auth: '<token>', // optional; if authentication is needed (e.g. private repo), oAuth: '<token>' could be used instead. 'auth' will be prioritized
	}
});

console.log(notifier.update);
```


## API

### notifier = updateNotifier(options)

Checks if there is an available update. Accepts options defined below. Returns an instance with an `.update` property there is an available update, otherwise `undefined`.

### options

Type: `Object`

#### _OPTIONS SPECIFIC TO THIS PACKAGE_

#### repo

Type: `Object`

##### type

Type: `string`
Description: A string that identifies the kind of repo. Is used to reference the function to check for latest tag/version. Standard options are npm, gitlab, github (will default to npm if not set). Custom types can be used.

##### url

Type: `string`
Description: Defaults to the standard hosts if not set.

##### auth or oAuth

Type: `string`
Description: Auth or OAuth token.

#### repoChecks

Type: `Object`

##### <custom type>

*Required if custom type is used for repo.type*<br>
Type: `function`
Return: `Promise.<string>`
Description: Returns a promise the will resolve to the latest version/tag.


#### _GENERAL OPTIONS_
#### pkg

Type: `Object`

##### name

*Required*<br>
Type: `string`

##### version

*Required*<br>
Type: `string`

#### updateCheckInterval

Type: `number`<br>
Default: `1000 * 60 * 60 * 24` *(1 day)*

How often to check for updates.

#### callback(error, update)

Type: `Function`

Passing a callback here will make it check for an update directly and report right away. Not recommended as you won't get the benefits explained in [`How`](#how). `update` is equal to `notifier.update`.

#### shouldNotifyInNpmScript

Type: `boolean`<br>
Default: `false`

Allows notification to be shown when running as an npm script.

### notifier.notify([options])

Convenience method to display a notification message. *(See screenshot)*

Only notifies if there is an update and the process is [TTY](https://nodejs.org/api/process.html#process_tty_terminals_and_process_stdout).

#### options

Type: `Object`

##### defer

Type: `boolean`<br>
Default: `true`

Defer showing the notification to after the process has exited.

##### message

Type: `string`<br>
Default: [See above screenshot](https://github.com/yeoman/update-notifier#update-notifier-)

Message that will be shown when an update is available.

##### isGlobal

Type: `boolean`<br>
Default: auto-detect

Include the `-g` argument in the default message's `npm i` recommendation. You may want to change this if your CLI package can be installed as a dependency of another project, and don't want to recommend a global installation. This option is ignored if you supply your own `message` (see above).

##### boxenOpts

Type: `Object`<br>
Default: `{padding: 1, margin: 1, align: 'center', borderColor: 'yellow', borderStyle: 'round'}` *(See screenshot)*

Options object that will be passed to [`boxen`](https://github.com/sindresorhus/boxen).


## About

The idea for this module came from the desire to apply [update-notifier][un] to repositories.
I then found [update-notifier-plus][unp] which implemented this for github.
Because I am using GitLab as well as GitHub I decided to write a package for that along the lines of [update-notifier-plus][unp].
In the process I realized that [update-notifier-plus] was not completely up to date and i asked myself what happens in the future if I want to use this for a work project, where Bitbucket is used. 
Therefore i also implemented the option to pass custom functions to check for latest tags or even overwrite the ones for npm, github and gitlab.


## Credits

I have to give credit to obviously the original [update-notifier][un], which I forked. Also to [update-notifier-plus][unp], which I took some inspiration from.


## License

BSD-2-Clause © Google

[un]: https://www.npmjs.com/package/update-notifier
[unp]: https://www.npmjs.com/package/update-notifier-plus


