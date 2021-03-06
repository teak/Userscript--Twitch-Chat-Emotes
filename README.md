# [Homepage](http://cletusc.github.io/Userscript--Twitch-Chat-Emotes/)

This is the build guide and source for the [Emote Menu for Twitch](http://cletusc.github.io/Userscript--Twitch-Chat-Emotes/). Normal users should check out the homepage as it is a lot more friendly. For developers that want to contribute, check out the build guide below, make some neat changes, then open a pull request.

# Building

In order to speed up building and testing, this project uses [npm](https://www.npmjs.org/) and [gulp](http://gulpjs.com/) to build the script.

### Basics

1. Make sure npm is installed as per their docs.
2. Run `npm install` to get the latest dependencies.
3. Make sure `gulp` is globally installed: `npm install -g gulp`.
4. Make changes to the script in the `src` folder (or other files as shown in the locations section below).
5. Run `gulp` to build the compiled script.

### Source files

- Userscript metadata: All userscript metadata is found in `package.json` within the `userscript` property and must be valid JSON.
- Templates: All templates are found in `src/templates` and are compiled with [hogan.js](http://twitter.github.io/hogan.js/) using [mustache syntax](http://mustache.github.io/mustache.5.html). Templates are accessible within `src/script.js` by referencing `templates.filename();`. For a filename of `foo.html`, you can reference it by `templates.foo();`. You may pass in data to the template like so: `templates.foo({bar: 'baz'});`. All templates are automatically referenced as partials for all other templates.
- Styles: All CSS styles are found in `src/styles`. When adding a new file, you must also add the file to `gulp/files.js` in the correct order.
- News updates: All news updates should be put in `news.json` and must be valid JSON. The key must be a timestamp in the form of `YYYY-MM-DDTHH:MM`, e.g. `2014-02-05T15:07` and the value will be your message. All links must have `target="_blank"`.

### Third-party files

This script uses a few third-party files to make things easier. All third-party files such as styles or javascript (including jQuery plugins) are minified with the source filename included. Version information can be found in the `package.json`. For any minified javascript files that are found in `src`, these are files that will eventually be split off from this repo.

Adding a third-party file is very simple.

1. Add the package to the `package.json`. For packages without a package.json, add them under the `napa` property.
2. Run `npm install`.
3. Add the files that need to be included to `gulp/files.js` in the correct order. Typically files will be located in `node_modules`.

Each time you compile the script, the third party files will automatically be included.

### Compiled files

- `script.user.js`: This is the userscript file which has all of the required metadata to work with the various userscript engines.
- `script.min.js`: This is the browser-ready minified version of the script. This is ideal for referencing by third-party scripts. Third-party scripts should only check for updates directly from Github once every 24 hours; do not directly reference the Github file. For direct linking, you should mirror this script to a CDN and use that instead.

### Gulp commands

- `gulp`: Compiles the script.
- `gulp watch`: Watches for changes to `package.json` and files in `src/` and automatically runs `gulp`.
- `gulp init`: Creates a simple `build/config/script-copy-paths.json`. Every time you compile, the compiled userscript will be copied to all paths in this file. This allows you to easily test the userscript using your normal engine (Greasemonkey or others). With `gulp watch`, all you need to do is make the changes in the source, and refresh your browser! An example file might look like this:

```json
[
	{
		"dir": "D:/AppData/Mozilla Firefox/Profiles/default/gm_scripts/Twitch_Chat_Emotes",
		"filename": "script.user.js"
	}
]
```

### Windows developers

Due to a limit of folder nesting in Windows, more specifically length of the path, you may be unable to delete the `node_modules` folder. This is a problem that should be fixed in Windows, `npm` or `node` overall, but for our purposes is caused by the `lodash` module, which is used in `gulp`. The easiest method that I have found to clear the `node_modules` folder is to use [rimraf](https://www.npmjs.org/package/rimraf), however it can be destructive, so be careful and check for typos before you send the commands.

1. Browse to the project folder.
2. Install rimraf: `npm install rimraf -g`
3. Clear the node_moules folder: `rimraf node_modules`

Some reading on the matter:

- [joyent/node#6960](https://github.com/joyent/node/issues/6960)
- [lodash/lodash#501](https://github.com/lodash/lodash/issues/501)
- [gulpjs/gulp-util#23](https://github.com/gulpjs/gulp-util/pull/23)
