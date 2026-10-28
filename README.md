# Meow Studio website

GitHub Pages publishes the main branch root, without a build step.

- / : studio introduction and app catalog
- /apps/insta-download/ : Insta Download introduction and support
- /apps/meow-pair/ : Meow Pair introduction and support
- /assets/ : app icons
- /style.css : shared responsive styling
- /app-ads.txt : shared AdMob publisher authorization; keep this root path stable

To add an app, create apps/<stable-slug>/index.html, add its icon to assets/, then add a homepage catalog entry. Use absolute asset paths, verified app details, and app-specific store, support, and legal links. Avoid claiming an unverified release status.

Existing policy URLs remain in their separate repositories: /insta-download-legal/ and /meow-duo-legal/.

This website repository is separate from the Flutter and Unity repositories. The former Sites project is not the deployment source.
