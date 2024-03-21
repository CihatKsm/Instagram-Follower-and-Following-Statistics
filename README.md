# 🪄 Get Instagram Follower and Following Statistics
### ⚠️ The user is entirely responsible for the use of this code. The codes are clear and editable. No responsibility can be assumed on my behalf in this regard.
🔵 To use the code, first go to Instagram's official website from your browser normally and log in. Open the developer tools in your browser, paste the following code into the console section and press enter. You can get information from the published articles.

```javascript
//First we get the application id
const appIdLocation = document.body.innerHTML?.indexOf('"X-IG-App-ID":');
const appId = document.body.innerHTML?.slice(appIdLocation, appIdLocation + 100)?.split(',')[0]?.replaceAll(`"`, '')?.split(':')[1];

//Now we get the user id
const userIdLocation = document.body.innerHTML?.indexOf('"appScopedIdentity":');
const userId = document.body.innerHTML?.slice(userIdLocation, userIdLocation + 100)?.split(',')[0]?.replaceAll(`"`, '')?.split(':')[1];

//Now we are getting your followers and following
let followers = [], followersChecked = false;
let following = [], followingChecked = false;

//Getting followers list and posting list
function getFollowers(max_id) {
    fetch(`https://www.instagram.com/api/v1/friendships/${userId}/followers/?count=12${max_id ? ('&max_id=' + max_id) : ''}&search_surface=follow_list_page`, {
        "headers": {
            "x-ig-app-id": appId,
        },
        "method": "GET",
    }).then((data) => data?.json()).then((data) => {
        followers = [...followers, ...data?.users];
        if (data.next_max_id) getFollowers(data.next_max_id);
        else followersChecked = true;
    })
}

//Getting following list and posting list
function getFollowing(max_id) {
    fetch(`https://www.instagram.com/api/v1/friendships/${userId}/following/?count=12${max_id ? ('&max_id=' + max_id) : ''}&search_surface=follow_list_page`, {
        "headers": {
            "x-ig-app-id": appId,
        },
        "method": "GET",
    }).then((data) => data?.json()).then((data) => {
        following = [...following, ...data?.users];
        if (data.next_max_id) getFollowing(data.next_max_id);
        else followingChecked = true;
    })
}

//Starting control
getFollowers();
getFollowing();

console.clear();
console.log('##################################')
console.log('Control started!');

let interval1 = setInterval(() => {
    console.log('- Number of followers found:', followers.length);
    console.log('- Number of followers found:', following.length);

    if (followersChecked && followingChecked) {
        clearInterval(interval1);
        console.clear();
        console.log('Finished control');

        //Fixing data
        followers = followers.map(f => f.username);
        following = following.map(f => f.username);

        //Regenerating data
        const x = following.filter(f => !followers.includes(f));
        const y = followers.filter(f => !following.includes(f));

        //Output
        console.log('People you follow but they don\'t follow you:', x);
        console.log('Those who follow you but you don\'t follow:', y);
    }
}, 1000);
```
