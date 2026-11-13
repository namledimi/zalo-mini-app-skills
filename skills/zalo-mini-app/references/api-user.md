# User & Authorization APIs

## authorize
Request permission for sensitive data.
```js
import { authorize } from "zmp-sdk/apis";

try {
  const granted = await authorize({ scopes: ["scope.userInfo", "scope.userPhonenumber"] });
  // granted: { "scope.userInfo": true, "scope.userPhonenumber": false }
} catch (error) {
  if (error.code === -201) {
    // User refused the permission dialog
  }
}
```

Call `authorize` only from a user action (a tap), not when a screen opens.

Scopes:
- `scope.userInfo` - Name & avatar
- `scope.userPhonenumber` - Phone number
- `scope.userLocation` - GPS location

## getUserID
Get unique user identifier (no permission required).
```js
import { getUserID } from "zmp-sdk/apis";

const userID = await getUserID({});
```

## getUserInfo
Get user profile with name and avatar.
```js
import { getUserInfo } from "zmp-sdk/apis";

const { userInfo } = await getUserInfo({
  autoRequestPermission: true,  // Auto-show permission dialog
  avatarType: "normal"          // "small" | "normal" | "large"
});

// userInfo: { id, name, avatar, followedOA, idByOA, isSensitive }
```

Handle permission denied (`-1401` is specific to `getUserInfo`; `authorize` and `followOA` use `-201`):
```js
try {
  const { userInfo } = await getUserInfo();
} catch (error) {
  if (error.code === -1401) {
    // User denied permission
  }
}
```

## getPhoneNumber
Get a one-time token for the user's phone number (requires `scope.userPhonenumber`).
The number itself is not returned to the Mini App: `number` is deprecated.
```js
import { getPhoneNumber, getAccessToken } from "zmp-sdk/apis";

const { token } = await getPhoneNumber({});
const accessToken = await getAccessToken({});
// Send both to your server. The server calls Zalo's Open API
// (https://graph.zalo.me/v2.0/me/info with headers access_token, code=token, secret_key)
// to read the number. The token works once and expires after 2 minutes.
```

## getAccessToken
Get token for backend verification.
```js
import { getAccessToken } from "zmp-sdk/apis";

const { accessToken } = await getAccessToken({});
// Send to backend for verification via Open API
```

## getSetting
Check current permission status.
```js
import { getSetting } from "zmp-sdk/apis";

const settings = await getSetting({});
// { "scope.userInfo": true, "scope.userPhonenumber": false }
```

## getAppInfo
Get Mini App information.
```js
import { getAppInfo } from "zmp-sdk/apis";

const { appId, appName, version } = await getAppInfo({});
```

## getSystemInfo
Get device and Zalo app info.
```js
import { getSystemInfo } from "zmp-sdk/apis";

const {
  platform,        // "android" | "ios"
  zaloVersion,
  language,
  screenWidth,
  screenHeight,
  statusBarHeight,
  safeAreaInsets
} = await getSystemInfo({});
```

## getDeviceIdAsync
Get unique device identifier.
```js
import { getDeviceIdAsync } from "zmp-sdk/apis";

const deviceId = await getDeviceIdAsync({});
```

## getContextAsync
Get context when Mini App was opened.
```js
import { getContextAsync } from "zmp-sdk/apis";

const context = await getContextAsync({});
// { entryPoint, referrer, ... }
```
