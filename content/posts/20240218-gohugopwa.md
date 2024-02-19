---
title: "Upgrading this Blog to be a Progressive Web App"
image: "/posts/images/posts/20240218-gohugopwa/1.jpeg"
date: 2024-02-18T18:34:35+01:00
draft: false
tags: ["pwa", "hugo", "development", "chatgpt", "stable diffusion"]
---

Scrolling through the depths of the web I encountered something called PWA. Then I found a Blog which used such technology. Looked really pretty without the browser control on a mobile device, so I wanted to have that too. In this blog post, we'll explore the process of transforming a Hugo-based website into a PWA.

### Understanding the Basics

At its core, a PWA requires two essential components: a `manifest.webmanifest` file and a `serviceworker.js` file. The `manifest.webmanifest` file contains metadata about the application, such as its name, description, and icons, while the `serviceworker.js` file handles tasks like caching assets and enabling offline functionality.

### Manifest Configuration

Let's delve into the configuration of the `manifest.webmanifest` file:

```json
{
    "lang": "en-GB",
    "dir": "ltr",
    "name": "A name",
    "short_name": "A shorter name",
    "description": "A description",
    "start_url": "/",
    "display": "fullscreen",
    "orientation": "portrait",
    "background_color": "#ffffff",
    "theme_color": "#000000",
    "icons": [
        {
            "src": "/android-icon-192x192.png",
            "sizes": "192x192",
            "type": "image/png"
        }
    ]
}
```

### Integrating into Hugo Templates

To integrate the PWA functionalities into your Hugo templates, you'll need to add the following code snippets to your HTML:

#### Meta Tags

```html
<!-- Apple specific meta tags -->
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<!-- Generic meta tags -->
<meta name="mobile-web-app-capable" content="yes">
<meta name="mobile-web-app-status-bar-style" content="black" />
<meta name="theme-color" media="(prefers-color-scheme: light)" content="#f6eeea" />
<meta name="theme-color" media="(prefers-color-scheme: dark)" content="#494f5c" />

<!-- Link to manifest file -->
<link rel="manifest" href="{{ '/manifest.webmanifest' | absURL }}">

<!-- Link to service worker JavaScript file -->
{{ $js := resources.Get "js/service-worker.js" }}
<script type="application/javascript" src="{{ $js.Permalink | absURL }}"></script>
```

#### Service Worker Registration

```javascript
navigator.serviceWorker && navigator.serviceWorker
    .register('/service-worker.js').then(function(registration) { 
        console.log('Excellent, registered with scope: ', registration.scope);
    })
```

### File Placement

Ensure the following files are placed in their respective directories within your Hugo theme:

- `themes/theme-name/assets/js/service-worker.js`
- `themes/theme-name/static/manifest.webmanifest`
- `themes/theme-name/layouts/partials/header.html`

### Verification and Testing

Once integrated, you can verify the functionality using browser developer tools. Under the Application tab, you should be able to see the manifest and service worker registered successfully.

### Apple, EU and PWA

Additionally, it's worth noting recent developments in the realm of web apps, particularly concerning iOS 17.4 beta in Apple's ecosystem. Reports suggest that changes introduced by Apple might have implications for web apps, potentially affecting their functionality on iOS devices. For further details on this matter, you can refer to the insightful analysis provided in the blog post titled "Did Apple Just Break Web Apps in iOS 17.4 Beta?" on Open Web Advocacy's website. This discussion sheds light on the potential impact of Apple's updates on web app developers and underscores the importance of staying informed about evolving technology landscapes.

### Conclusion

By converting a blog to support PWA we achive a sleek design with a more native APP like experience and potential offline support which probably will not work in the near future on an iOS device in the EU. Oh well...

### References

For further insights and detailed implementation, refer to the sources provided below:

- [GitHub Gist: Hugo PWA Integration](https://gist.github.com/sconstantinides/221a9ae6bf8a1d2bc02f1e5d5d5ddf61)
- [Make Your PWAs Look Handsome on iOS](https://itnext.io/make-your-pwas-look-handsome-on-ios-fd8fdfcd5777)
- [MDN Web Docs: Theme Color Meta Tag](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta/name/theme-color)
- [YouTube Tutorial: Creating PWAs](https://www.youtube.com/watch?v=PpT8JE5BbZI)
- [Did Apple just break Web Apps in iOS 17.4 Beta (EU)?](https://open-web-advocacy.org/blog/did-apple-just-break-web-apps-in-ios17.4-beta-eu/)
