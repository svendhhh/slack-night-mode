# Slack Night Mode
A user style for easy Slack theming. [CC0](http://creativecommons.org/publicdomain/zero/1.0/).

## Usage

### Windows Desktop App 4.0
First of all, make sure your slack are on the latest version (4.0.0)
Open Command prompt, navigate to %localappdata%/slack/app-4.0.0/resources/
Install npx with `npm install -g npx`

Unpack the app.asar with the command:`npx asar extract app.asar app.asar.unpacked`

Insert the following code in the unpacked file `ssb-interop.bundle.js` 

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

Repack the asar file: `npx asar pack app.asar.unpacked app.asar`

## Themes

### Supported

#### Black ([source](scss/main.scss) - [build](css/black.css) - [install](https://userstyles.org/styles/117475/slack-night-mode-black))

The primary supported theme. This is an excellent theme if you use a program like f.lux or redshift.
