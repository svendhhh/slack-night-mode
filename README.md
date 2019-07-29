# Slack Night Mode
A user style for easy Slack theming. [CC0](http://creativecommons.org/publicdomain/zero/1.0/).

## Usage

## Our instructions

### Windows Desktop App 4.0

0) Make sure you have NPM installed or install it (https://blog.teamtreehouse.com/install-node-js-npm-windows)
1) Make sure your slack are on the latest version (4.0.1)

2) Open Command prompt, navigate to %localappdata%/slack/app-4.0.1/resources/

3) Install npx with `npm install -g npx`
4) Install Asar with `npm install -g asar`

5) Unpack the app.asar with the command: `npx asar extract app.asar app.asar.unpacked`

6) Insert the below code at the end of in the unpacked file `ssb-interop.bundle.js` 
Note: It may look like mumbo-jumbo in the file, but it's just minified javascript - do not despair.

7) Repack the asar file: `npx asar pack app.asar.unpacked app.asar`

```
document.addEventListener("DOMContentLoaded", function() {
    let webviews = document.querySelectorAll(".TeamView webview");
    const cssPath = 'https://raw.githubusercontent.com/svendhhh/slack-night-mode/master/css/raw/black.css';
    let cssPromise = fetch(cssPath).then(response => response.text());
    cssPromise.then(css => {
       let s = document.createElement('style');
       s.type = 'text/css';
       s.innerHTML = css;
       document.head.appendChild(s);
    });    
    webviews.forEach(webview => {
       webview.addEventListener('ipc-message', message => {
          if (message.channel == 'didFinishLoading')       
             cssPromise.then(css => {
                let script = `
                      let s = document.createElement('style');
                      s.type = 'text/css';
                      s.id = 'slack-custom-css';
                      s.innerHTML = \`${css}\`;
                      document.head.appendChild(s);
                      `
                webview.executeJavaScript(script);
             })
       });
    });
 });
```

---


## Original Readmy from laCour's project:


### Browser

This theme requires that you use [a user styles extension](https://github.com/openstyles/stylus/wiki/Stylish-Alternatives) for your browser, such as Stylus (available for [Firefox](https://addons.mozilla.org/en-US/firefox/addon/styl-us/), [Chrome](https://chrome.google.com/webstore/detail/stylus/clngdbkpkpeebahjckkjfobafhncgmne), and [Opera](https://addons.opera.com/en/extensions/details/stylus/)).

### Desktop App

No official support. Workarounds exist.
 - [dark-slack](https://github.com/calebboyd/dark-slack) cli
 - [slack-theme-cli](https://github.com/mykeels/slack-theme-cli)

**🛑 READ FIRST:** Most workarounds will request the compiled CSS file from this repository. You are strongly discouraged from using a remote CSS file. It's recommended that you create your own copy. An XSS attack could put your Slack client at risk.

[![Chat on Gitter](https://badges.gitter.im/laCour/slack-night-mode.png)](https://gitter.im/slack-night-mode/Lobby?utm_source=share-link&utm_medium=link&utm_campaign=share-link) ([previous discussion](https://github.com/laCour/slack-night-mode/issues/73#issuecomment-242707078))

## Themes

### Supported

#### Black ([source](scss/main.scss) - [build](css/black.css) - [install](https://userstyles.org/styles/117475/slack-night-mode-black))

The primary supported theme. This is an excellent theme if you use a program like f.lux or redshift.

![Black Screenshot](https://userstyles.org/style_screenshots/117475_after.png)

#### Aubergine ([source](scss/themes/_aubergine.scss) - [build](css/variants/aubergine.css) - [install](https://userstyles.org/styles/101971/slack-night-mode))

This is based on Slack's aubergine/maroon style. It's the original theme.

![Aubergine Screenshot](https://userstyles.org/style_screenshots/101971_after.png)

### Variants

* **Arc ([source](scss/themes/_arc-dark.scss) - [build](css/variants/arc-dark.css))** _by [@Lemmmy](https://github.com/Lemmmy)_
* **Gruvbox Dark ([source](scss/themes/_gruvbox-dark.scss) - [build](css/variants/gruvbox-dark.css))** _by [@lvarado](https://github.com/lvarado)_
* **Midnight Blue ([source](scss/themes/_midnight-blue.scss) - [build](css/variants/midnight-blue.css))** _by [@matt-h](https://github.com/matt-h)_
* **Solarized Dark ([source](scss/themes/_solarized-dark.scss) - [build](css/variants/solarized-dark.css))** _by [@glostis](https://github.com/glostis)_
* **Solarized Light ([source](scss/themes/_solarized-light.scss) - [build](css/variants/solarized-light.css))** _by [@glostis](https://github.com/glostis)_
* **Tomorrow Dark (base16) ([repository](https://github.com/danarnold/slack-night-mode))** _by [@danarnold](https://github.com/danarnold)_

### Extensions

Variants can have extensions which add additional changes.

#### Monospaced ([source](scss/themes/_monospaced.scss))

Replaces the messaging font stack with a monospace font stack.
