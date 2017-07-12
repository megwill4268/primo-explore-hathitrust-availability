# primo-explore-hathitrust-availability
An example implementation of a custom Angular component in the new Primo UI

## Purpose
During the [Primo Hackathon](http://igelu.org/archives/9618), we started discussing best practices for sharing custom Primo components / directives via NPM packages. This is not intended to serve as a definitive list of best practices. Rather, the goal is to seed the conversation with some concrete examples of how we might: 
  - avoid overwriting the built-in 'after' components by defining child components that can be appended to an 'after' component.
  - move external API calls out of controllers and into custom services
  - create unit tests for services and component controllers 
  - expose configurable options via directive attributes

## Features
When search results are displayed, a record's OCLC numbers are passed to the [HathiTrust Bib API](https://www.hathitrust.org/bib_api). If at least one item with free full-text access is found, a link to the HathiTrust record is appended to the availability section. 

### Screenshot
![screenshot](screenshots/screenshot.png)

## Usage
Once the module is installed in your [customization environment](https://github.com/ExLibrisGroup/primo-explore-devenv), `HathiTrustAvailability` as a dependency for your custom module, then add the HathiTrust availability component to the `prmSearchResultAvailabilityLineAfter` component. For example:

```javascript
var app = angular.module('viewCustom', ['hathiTrustAvailability']);

app.component('prmSearchResultAvailabilityLineAfter', {
    template: '<hathi-trust-availability></hathi-trust-availability>'
  });
```
(Note: If you're using the `--browserify` build option, you will need to first import the module with `require('primo-explore-hathitrust-availability);` or `import 'primo-explore-hathitrust-availability';`.) 

The idea here is to allow multiple custom components to be added to the `prmSearchResultAvailabilityLineAfter` rather than attempting to redefine it. 

By default, the component will display full-text links for any resource. If you want it avoid looking for full-text availability on records for which you already have an online copy, you can add a `hide-online=tue` attribute to the component: 

```html
<hathi-trust-availability hide-online="true"></hathi-trust-availability>
```

The default availability message is "Full Text Available at HathiTrust". You can override this by setting the `msg` attribute:

```html
<hathi-trust-availability hide-online="true" msg="WOW, HathiTrust! Lucky you!"></hathi-trust-availability>
```

### Updating the icon location
By default, this module will look for the HathiTrust icon in `custom/CENTRAL_PACKAGE/img/hathitrust.svg`. If you are using a view-level package, you will need to update the icon location like so: 

```javasctipt
app.value('hathiTrustIconPath', '/custom/CENTRAL_PACKAGE/img/hathitrust.svg');
```

```javascript

## Running Tests
1. Clone the repo
2. Run `npm install`
3. Run `npm test`



