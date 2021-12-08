# Star Wars API Demo

![Peek 2021-12-08 09-12](https://user-images.githubusercontent.com/74467166/145206831-596087cc-9c65-41f5-ac08-4180ceca29f5.gif)

## About

This is a Vue.js example project in order to showcase some of my front-end skills, documentation following capacity, repository management, and quick learning capability. That's because there was not a single follow-through tutorial and I had to fit many different puzzle pieces to make this project work in the way I intended.

The program fetches characters from [Star Wars Api (Swapi)], processes the response JSON object and return data about the given character. For instance, which Star Wars movies that the character has been in. It can also use search engines and websites through API fetching to fetch a character image.

[Star Wars Api (Swapi)]: https://swapi.dev/

## Image Search

If provided Microsoft Azure (Bing) and Custom Configuration API keys in a '.env' file, the program will make a custom image search according to search engines and websites provided on your Azure portal custom configuration. If the necessary keys for the image search are not provided, it will simply not show any character image.

Image Search API Resources: <br />
[Azure API] <br />
[Custom Configuration] <br />
[Bing Custom Search Portal] <br />

[Azure Api]: https://docs.microsoft.com/en-us/bing/search-apis/bing-custom-search/how-to/quick-start
[Custom Configuration]: https://docs.microsoft.com/en-us/azure/cognitive-services/bing-custom-search/quick-start
[Bing Custom Search Portal]: https://customsearch.ai/

## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```

### Compiles and minifies for production
```
yarn build
```

### Lints and fixes files
```
yarn lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
