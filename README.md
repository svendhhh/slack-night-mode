# Slack Night Mode
A user style for easy Slack theming. [CC0](http://creativecommons.org/publicdomain/zero/1.0/).

## Usage

### Windows Desktop App 4.0

0) Make sure you have NPM installed or install it (https://blog.teamtreehouse.com/install-node-js-npm-windows)
1) Make sure your slack are on the latest version (4.0.0)

2) Open Command prompt, navigate to %localappdata%/slack/app-4.0.0/resources/

3) Install npx with `npm install -g npx`
4) Install Asar with `npm install -g asar`

5) Unpack the app.asar with the command: `npx asar extract app.asar app.asar.unpacked`

6) Insert the following code at the end of in the unpacked file `ssb-interop.bundle.js` 
Note: It may look like mumbo-jumbo in the file, but it's just minified javascript - do not despair.
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

7) Repack the asar file: `npx asar pack app.asar.unpacked app.asar`

## Themes

### Supported

#### Black ([source](scss/main.scss) - [build](css/black.css) - [install](https://userstyles.org/styles/117475/slack-night-mode-black))

The primary supported theme. This is an excellent theme if you use a program like f.lux or redshift.
