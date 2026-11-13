# Zalo Platform APIs

## Official Account (OA)

### followOA / unfollowOA
Prompt user to follow/unfollow OA.
```js
import { followOA, unfollowOA } from "zmp-sdk/apis";

try {
  await followOA({ id: "your-oa-id" });
} catch (error) {
  if (error.code === -201) {
    // User declined to follow
  }
}
await unfollowOA({ id: "your-oa-id" });
```

Check whether the user already follows with `getUserInfo().userInfo.followedOA`.

### interactOA
Open OA interaction options. Note: this one takes `oaId`, unlike `followOA` / `viewOAQr`.
```js
import { interactOA } from "zmp-sdk/apis";

await interactOA({ oaId: "your-oa-id" });
```

### viewOAQr
Display OA QR code.
```js
import { viewOAQr } from "zmp-sdk/apis";

viewOAQr({ id: "your-oa-id" });
```

## Chat & Social

### openChat
Open chat with user or OA.
```js
import { openChat } from "zmp-sdk/apis";

openChat({
  type: "oa",  // "user" | "oa"
  id: "oa-or-user-id",
  message: "Hello!"
});
```

### openProfile
Open user/OA profile.
```js
import { openProfile } from "zmp-sdk/apis";

openProfile({
  type: "user",
  id: "user-id"
});
```

### openProfilePicker
Open friend selector.
```js
import { openProfilePicker } from "zmp-sdk/apis";

const { users } = await openProfilePicker({
  maxSelection: 5
});
```

### openShareSheet
Share content.
```js
import { openShareSheet } from "zmp-sdk/apis";

openShareSheet({
  type: "link",
  data: {
    link: "https://zalo.me/s/app-id",
    title: "Check this out!",
    description: "Amazing mini app",
    thumbnail: "https://example.com/thumb.jpg"
  }
});
```

### openPostFeed
Post to Zalo feed.
```js
import { openPostFeed } from "zmp-sdk/apis";

openPostFeed({
  type: "link",
  data: {
    link: "https://example.com",
    title: "My Post"
  }
});
```

## App Features

### createShortcut
Add shortcut to home screen.
```js
import { createShortcut } from "zmp-sdk/apis";

await createShortcut({});
```

### addRating
Prompt user to rate mini app.
```js
import { addRating } from "zmp-sdk/apis";

addRating({});
```

### minimizeApp
Minimize to background.
```js
import { minimizeApp } from "zmp-sdk/apis";

minimizeApp({});
```

### favoriteApp
Add to favorites.
```js
import { favoriteApp } from "zmp-sdk/apis";

favoriteApp({});
```
