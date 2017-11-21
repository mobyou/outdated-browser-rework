# Outdated Browser Rework

Detects outdated browsers and advises users to upgrade to a new version. Handles mobile devices!

This is a fork of [Burocratik](http://www.burocratik.com)'s excellent Outdated Browser, adding a number of new features.

One of the challenges with making this type of module is that the display can't use any current tech - so yes, this is hard. This module is tested all the way back to IE6.

This module does not need jQuery.

#### Version 2 notes 

 - New translations
 - New `false` option to disable browser support.
 - IE default to `false`. You can still specify `6` to `11` if, for some reason, you still support IE in 2017.
 - SCSS file is named `index.scss` for use with `sass-npm`
 - CSS file is included
 - Update `ua-parser-js` to fix parsing some more esoteric UAs 

## Usage

### JS

	var outdatedBrowserRework = require("outdated-browser-rework");

	outdatedBrowserRework();

If you like, specify options, eg:

	outdatedBrowserRework({
		browserSupport: {
			'Chrome': 57, // Includes Chrome for mobile devices
			'Edge': 38,
			'Safari': 10,
			'Mobile Safari': 10,
			'Firefox': 58,
			// You could specify a version here if you still support IE in 2017.
			// You could also instead seriously considerer what you're doing with your time and budget
			'IE': false, 
		}
	});

See below for more options.

Browsers that are __older__ than the versions supplied will see a message, depending on their platform:

 - On desktop browsers, users will be directed to [outdatedbrowser.com](http://outdatedbrowser.com)
 - on iOS devices, users will be asked to visit the Settings app and upgrade their OS.
 - On Android devices, users will be directed to Chrome in Google Play.



#### Options

 - __browserSupport__:Object - A matrix of browsers and their major versions - see above for demo. Anything less will be unsupported.
 - __requiredCssProperty__:String - A CSS property that must be supported.
 - __requireChromeOnAndroid__:Boolean - Ask Android users to install Chrome.

### SCSS

	@import "vendor/outdated-browser-rework.scss";

### HTML

	<div id="outdated"></div>

## Integration Tips

We normally concatenate and combine different JS files using `npm` and `browserify`, but it's best to bundle this particular package by itself, since other scripts may expect things like `console` and `function.bind()` to exist and won't work on old browsers - if you bundle this with other software, and an old browser tried to use the bundle, the JS will probably fail before outdated-browser has a chance to do any work.

### In your template

In `<head>`, before any other `script` tags:

    <script src="/js/dist/oldbrowser.js"></script>

### In `oldbrowser.js`

Start `outdated-browser-rework` and call it with your preferred options:

    var outdatedBrowserFork = require("outdated-browser-rework");

    outdatedBrowserFork({
    	browserSupport: {
    		'Chrome': 37,
    		'IE': 13,
    		'Safari': 7,
    		'Mobile Safari': 7,
    		'Firefox': 32
    	},
    	requireChromeOnAndroid: true
    })


### In your gulpfile

Add the following underneath your existing `js` task:

		gulp
			.src('./public/js/src/oldbrowser.js')
			.pipe(browserify({
				debug : ! gulp.env.production
			}))
			.pipe(gulp.dest('./public/js/dist'))

Doing this will mean that `dist/oldbrowser.js` will only include `outdated-browser-rework` and its dependency `user-agent-parser`,without anything else to get in the way.

## Differences from Outdated Browser 1.1.0

 - Add explicit browser support via the __browserSupport__ option
 - Add mobile support. Users on iOS and Android will be directed to the Apple App Store and Google Play respectively.
 - Add new __requireChromeOnAndroid__ option
 - Be an NPM module
 - Use SASS (specifically SCSS)
 - No AJAX, languages are only 8K and removing the AJAX library has made the code substantially shorter.

And some code fixes:

 - Pass jshint
 - Remove HTML entities. It's 2015, we have unicode now.
 - Included Vanilla JS onload option - that way you can keep using jQuery 2 and not have to revert to 1.x just to show messages to old browsers.
 - Simplify some variable and function names

There's still some TODOs from the original code:

 - Try and eliminate IDs (they're JS globals, so EUW)
 - Move all styling into SCSS (need to test if this breaks old IEs)
 - Re-do Farsi (RTL) support from original Outdated Browser

## Author

This rework is made by Mike MacCana.
The original Outdated Browser is made with love at [Bürocratik](http://burocratik.com)
