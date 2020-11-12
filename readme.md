# Mangadex-api

[![NPM Version](https://img.shields.io/npm/v/mangadex-api.svg?style=flat-square)](https://www.npmjs.com/package/mangadex-api)
[![npm downloads](https://img.shields.io/npm/dm/mangadex-api.svg?style=flat-square)](http://npm-stat.com/charts.html?package=mangadex-api)

This is [Mangadex](https://mangadex.org) website api wrapper.

# Migration to version 4

Main focuses in V4 were [Typescript](https://www.typescriptlang.org/) and [Mangadex](https://mangadex.org/) [API V2](https://mangadex.org/thread/351011).  
So V1 support was dropped completely. If you still, for some reason, want to use API V1 then use `agent.callApi(endpoint, { baseUrl: 'https://mangadex.org/api' })` code snippet to request data from API V1.  

So there're new api groups:
- chapter/{id|hash}
- group/{id}
  - group/{id}/chapters
- manga/{id}
  - manga/{id}/chapters
  - manga/{id}/covers
- tag
- tag/{id}
- user/{id}
  - user/{id}/chapters
  - user/{id}/followed-manga
  - user/{id}/followed-updates
  - user/{id}/manga/{mangaId}
  - user/{id}/ratings
  - user/{id}/settings
- user/{id}/marker

Each of them is available on `Mangadex` instance via these properties:

```js
const client = new Mangadex()

// don't forget to login
await client.agent.login(username, password)

// and here we go

// chapter/{id|hash}
client.chapter.getChapter(chapterId)

// group/{id}
client.group.getGroup(groupId)

// group/{id}/chapters
client.group.getGroupChapters(groupId)

// manga/{id}
client.manga.getManga(mangaId)

// manga/{id}/chapters
client.manga.getMangaChapters(mangaId)

// manga/{id}/covers
client.manga.getMangaCovers(mangaId)

// tag
client.tag.getTags()

// tag/{id}
client.tag.getTag(tagId)

// user/{id}
client.user.getUser(userId)

// user/{id}/chapters
client.user.getUserChapters(userId)

// user/{id}/followed-updates
client.user.getUserFollowedUpdates(userId)

// user/{id}/manga/{mangaId}
client.user.getUserManga(userId, mangaId)

// user/{id}/ratings
client.user.getUserRatings(userId)

// user/{id}/settings
client.user.getUserSettings(userId)

// user/{id}/followed-manga
client.user.getUserFollowedManga(userId)

// user/{id}/marker
client.user.setUserChapterRead(userId, chapters, read)
```

# Migration to version 3

LRU cache was removed from package, so you'll have to implement caching by yourself.  
[Mangadex constructor](#Mangadex) now accepts only 2 options.  
[getManga](#getManga) and [getChapter](#getChapter) lost **normalize** argument, now it's on **by default**.

# Installation

```bash
npm i mangadex-api
```

Or

```bash
yarn add mangadex-api
```

# Example

```js
const { Mangadex } = require('mangadex-api')

Mangadex.getManga(22723).then(({ manga, chapter, group }) => {
  console.log(`Manga ${manga.title} has ${chapter.length} chapters.`)
  console.log(`And contributed by ${group.length} groups.`)
})

Mangadex.getChapter(8857).then((chapter) => {
  console.log(
    `Chapter title is "${chapter.title}" and it is ${chapter.chapter} chapter from ${chapter.volume} volume.`
  )
})

// currently requires authorization
Mangadex.search('senko').then((response) => {
  console.log(`Found ${response.titles.length} titles.`)
})

Mangadex.getHome().then((home) => {
  if (home.accouncement) {
    console.log(`New accouncement!\n${home.accouncement.text}`)
  }
  console.log(
    `Todays top manga by follows is: ${home.top_manga.follows[0].title}`
  )
  console.log(
    `Todays top chapter is from manga: ${home.top_chapters.day[0].title}`
  )
  console.log(
    `Latest chapter is from manga: ${home.latest_updates.all[0].title}`
  )
})

Mangadex.getGroup(12).then((group) => {
  console.log(
    `Group ${group.name} has ${group.stats.follows} followers and ${group.stats.total_chapters} total chapters uploaded!`
  )
})
```

# Authorization example

```js
const { Mangadex } = require('mangadex-api')

const client = new Mangadex()

await client.agent.login('username', 'password', false)

const result = await client.search('To Be Winner')

console.log(result) // { titles: [{ title: 'To Be Winner', ... }] }
```

# Cached session example

```js
// first you must save your session somewhere

await client.agent.login('username', 'password', false)

await client.agent.saveSession('/path/to/session'))

// now we can use it

await client.agent.loginWithSession('/path/to/session')

const me = await client.getMe())

console.log(me)
```


# API

This section is no more, use Typedoc generated site instead.

---

# Contact

[My telegram](https://t.me/ejnshtein) and a [group](https://t.me/nyaasi_chat) where you can as your questions or suggest something.