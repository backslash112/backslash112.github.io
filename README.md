# Flexible-Jekyll is a simple and clean theme for Jekyll

https://cunyang.me

## Features

- [Google Fonts](https://fonts.google.com/)
- [Font Awesome](http://fontawesome.io/)
- [Disqus](https://disqus.com/)
- [Analytics](https://analytics.google.com/analytics/web/)
- Support Emoji

## Installation:
```
// Install dependencies
docker run --rm -v "$PWD":/usr/src/app -w /usr/src/app ruby:2.5 bundle install

// Build website
docker run --rm -v "$PWD:/srv/jekyll" -it jekyll/jekyll jekyll build
docker commit a4703c61e03c jekyll/jekyll-gem
docker run --rm -v "$PWD:/srv/jekyll" -it jekyll/jekyll-gem jekyll build --watch

// Run the website locally
docker run -it --rm -p 4201:4201 -v "$PWD":/usr/src/app -w /usr/src/app node /bin/bash -c ' npm i http-server -g && http-server -p 4201'
```

## License

GNU General Public License v3.0
