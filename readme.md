# Google Analytics Vimeo Video Tracking
Tracking Vimeo Player Events with Google Analytics.

## Usage
Include the scripts in the body section of the HTML document, just before the `</body>` tag. You’ll need to be running on a web server instead of opening the file directly in your browser. Flash and JS security restrictions will prevent the API from working when run locally.

## Configuration of the Google Tag Manager
For this alternative version of vimeo.ga.js (vimeo.gtm.js) we will use the full power of the Google Tag Manager.
This means we will use an standard way of event tracking using macro's and datalayer events.

### Creating the essential Macro's
A macro in GTM is just a place where you can store values that we can access later on (wihtin tags and rules).
From within a macro we can capture and store values coming from different sources: cookies, referrer, URL, the GTM dataLayer and so on. 
The best description for macro's I've came across is that Macro's are the pipes connecting what happened on your website with your tracking management logic.
We will use this Macro functionality to push our Vimeo events (but infact it will work for all events) to Google Analytics or Universal Analytics.

Go ahead and create five macros (a macro for each of Google Analytics event parameters). 
The naming of the Macro's and standard rule we will setup makes it possible to reuse the same macros to track every type of GA event data, so after this your all set for the future. 

__Macro Name:__ Event Category -> Data Layer Variable: _eventCategory_
__Macro Name:__ Event Action -> Data Layer Variable: _eventAction_
__Macro Name:__ Event Label -> Data Layer Variable: _eventLabel_
__Macro Name:__ Event Value -> Data Layer Variable: _eventValue_
__Macro Name:__ Event Interaction -> Data Layer Variable: _eventInteraction_

### Creating the event tag
We now can make our Google Analytics event. new Google Analytics tag, but this time we well choose "event" as "Track Type". 
The GTM interface will give you a nice set of input boxes. In each input box, which map to a Google Analytics event parameter, click on "macro" ad select the corresponding macro you have just created. 

### Add the proper rule
Now create and add a rule (event: GAevent) to fire the Google Analytics event tag.

### Basic
```html
<iframe src="http://player.vimeo.com/video/22439234?api=1" width="640" height="390" frameborder="0" webkitAllowFullScreen mozallowfullscreen allowFullScreen></iframe>
<script src='https://ajax.googleapis.com/ajax/libs/jquery/1.4.3/jquery.min.js'></script>
<script src="path/to/vimeo.ga.min.js"></script>
```	
### With some options
```html
<iframe src="http://player.vimeo.com/video/22439234?api=1" width="640" height="390" frameborder="0" data-progress="true" data-seek="true" webkitAllowFullScreen mozallowfullscreen allowFullScreen></iframe>
<script src='https://ajax.googleapis.com/ajax/libs/jquery/1.4.3/jquery.min.js'></script>
<script src="path/to/vimeo.ga.min.js"></script>
```
The **data-progress** and **data-seek** attributes enable tracking of progress and skip events. 

The iframe embeds a Vimeo video player and allows Vimeo to serve an HTML5 player rather than a Flash player for mobile devices that do not support Flash.

## Requirements
* jQuery 1.4.3 or higher
* Google Analytics Tracking Code (asynchronous)
* The end user must be using a browser that supports the HTML5 postMessage feature. Most modern browsers support postMessage, though Internet Explorer 7 does not support it.

## Browser Support
Tested in Chrome (21), Firefox (15), Safari (5,6), IE (8,9). Also tested on iOS.

## Event Tracking
All player events are only tracked once. Restarting the video will not reset the event trackers.

### Default event trackers
* Category: Vimeo
* Action:
	* **Started video**: when the video starts playing.
	* **Paused video**: when the video is paused.
	* **Completed video**: when the video reaches 100% completion.
* Label: URL of embedded video on Vimeo.

#####Example
```js
_gaq.push(['_trackEvent', 'Vimeo', 'Started video', 'http://player.vimeo.com/video/22439234', undefined, true]);
_gaq.push(['_trackEvent', 'Vimeo', 'Paused video', 'http://player.vimeo.com/video/22439234', undefined, true]);
_gaq.push(['_trackEvent', 'Vimeo', 'Completed video', 'http://player.vimeo.com/video/22439234', undefined, true]);
```
### Progress event trackers

* Category: Vimeo
* Action:
	* **25% Progress**: when the video reaches 25% of the total video time.
	* **50% Progress**: when the video reaches 50% of the total video time.
	* **75% Progress**: when the video reaches 75% of the total video time.
* Label: URL of embedded video on Vimeo.

#####Example
```js
_gaq.push(['_trackEvent', 'Vimeo', 'Played video: 25%', 'http://player.vimeo.com/video/22439234', undefined, true]);
_gaq.push(['_trackEvent', 'Vimeo', 'Played video: 50%', 'http://player.vimeo.com/video/22439234', undefined, true]);
_gaq.push(['_trackEvent', 'Vimeo', 'Played video: 75%', 'http://player.vimeo.com/video/22439234', undefined, true]);
```
### Seek event tracker
* Category: Vimeo
* Action:
	* **Skipped video**: when the video is skipped forward or backward.
* Label: URL of embedded video on Vimeo.

#####Example
```js
_gaq.push(['_trackEvent', 'Vimeo', 'Skipped video forward or backward', 'http://player.vimeo.com/video/22439234', undefined, true]);
```

### Bounce rate
The event trackers do not impact bounce rate of the page which embeds the video. The value of the opt_noninteraction parameter is set to `true`.

## Issues
Have a bug? Please create an issue here on GitHub!

## Changelog
### 0.2 (February 3, 2013):
 * Code cleanup.
 * Fixed a bug in pause event tracking.
 * Updated documentation.

### 0.1 (October 12, 2012):
 * Initial release.

## License
Licensed under the MIT license.

Copyright (C) 2012-2013 Sander Heilbron, http://sanderheilbron.nl

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
