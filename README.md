# Instagram-follower-following-analysis

```javascript
const followerHTML = `instagram sayfasından aldığınız html kodu`;
const followingHTML = `instagram sayfasından aldığınız html kodu`;

const cheerio = require('cheerio');

const $follower = cheerio.load(followerHTML);
const $following = cheerio.load(followingHTML);

let follower = []
let following = []

$follower('div').each((i, element) => {
    const text = $follower(element).find('span').find('a').text();
    if (!follower.find(f => f == text) && text !== '') follower.push(text);
});

$following('div').each((i, element) => {
    const text = $following(element).find('span').find('a').text();
    if (!following.find(f => f == text) && text !== '') following.push(text);
});

const unFollowers = following.filter(f => !follower.find(ff => ff == f))
const dontFollowing = follower.filter(f => !following.find(ff => ff == f))

console.log('Beni takip etmeyenler:' + unFollowers)
console.log('Takip etmediklerim:' + dontFollowing)
```
