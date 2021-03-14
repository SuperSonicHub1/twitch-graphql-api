# Twitch's GraphQL API
> A documentation and schema scraping of Twitch's GraphQL API.

Want a Twitch API that doesn't require a signup, has types and documentation, and makes use of one of the most powerful ways to write an API? Continue reading.

## Endpoints
Two are currently available:
* https://gql.twitch.tv/gql
* https://api.twitch.tv/gql

Pick your poison, I guess; I prefer the former.

## Getting Your Client-ID
A `Client-ID` header is required to use the API, unless you're into errors:
```json
{
    "error": "Bad Request",
    "status": 400,
    "message": "The \"Client-ID\" header is missing from the request."
}
```
Luckily, Twitch bakes a singular ID into their HTML for some reason: `kimne78kx3ncx6brgo4mv6wki5h1ko`.
If you're afraid that it'll change someday and want a way to scrape it, run this RegEx pattern through their HTML:
```js
/"Client-ID":"(.*)","Content-Type"/
```

## Logging In
This is completely optional, but you can go through Twitch's OAuth handshake and get access to login-only features. See [youtube-dl's Twitch extractor](https://github.com/ytdl-org/youtube-dl/blob/879866a2304c3b0bbbb048feb4253431f0219aa3/youtube_dl/extractor/twitch.py) if you want an implemented example, as it's decently complex.

## Getting the Schema
This repository comes with both GraphQL and JSON representations of the schema dated 3/14/2021, but if you need to download it yourself, e.g. this repository is dead, use `get-graphql-schema`:
```sh
npm install -g get-graphql-schema
# Now get it in GQL...
npx get-graphql-schema https://gql.twitch.tv/gql --header Client-Id=kimne78kx3ncx6brgo4mv6wki5h1ko > schema.graphql
# or JSON!
npx get-graphql-schema https://gql.twitch.tv/gql --header Client-Id=kimne78kx3ncx6brgo4mv6wki5h1ko -j > schema.json
```
> BTW: If the schema on this repository is out-of-date, update it and the docs with `pnpm run build` and make a pull request.

## Reading the Docs
Luckily, Twitch did all the hard work of documenting the API, we just have to make it easliy readable. This repository hosts generated documentation via GitHub Pages. However, if you want to generate and host it yourself, use `graphqldoc`:
```sh
npm install -g graphqldoc
npx graphqldoc -e https://gql.twitch.tv/gql -x \"Client-Id: kimne78kx3ncx6brgo4mv6wki5h1ko\" -o ./docs
# Now host it! I prefer using Python 3's http.server.
python3 -m http.server --directory docs/
```
> BTW: If the schema on this repository is out-of-date, update it and the docs with `pnpm run build` and make a pull request.

## Tools Used
* [get-graphql-schema](https://github.com/prisma-labs/get-graphql-schema) makes getting Twitch's schema dead simple.
* [graphqldoc](https://github.com/CodeSignal/graphqldoc) creates pretty documentation for us to read through.

## Inspiration & Help
* [A Stack Overflow thread on scraping GraphQL schemas](https://stackoverflow.com/questions/37397886/get-graphql-whole-schema-query)
* [Maurice Wahba's own documentation of the API](https://github.com/mauricew/twitch-graphql-api)
    * If you want to find creative ways to use the API and get a lower level explanation, give it a read!
* [youtube-dl]()'s [Twitch extractor](https://github.com/ytdl-org/youtube-dl/blob/879866a2304c3b0bbbb048feb4253431f0219aa3/youtube_dl/extractor/twitch.py)

## License
This entire repository is licensed under the *I Solemny Swear That I Am Up To No Good Public License*, a modification of the [WTFPL](http://www.wtfpl.net/) by [d0nk](https://github.com/d0nk) for the release of [parler-tricks](https://github.com/d0nk/parler-tricks), effectively releasing this body of work into the public domain.
