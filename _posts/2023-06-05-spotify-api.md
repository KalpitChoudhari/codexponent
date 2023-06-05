---
layout: post
title:  "Spotify API"
author: Kalpit
categories: [ javascript, API ]
image: assets/images/spotify.png
---

## **Introduction:**
Spotify is a popular music streaming platform that offers a wide range of features for its users. One interesting aspect is the ability to retrieve information about the currently playing song. In this blog post, we will guide you through the steps to access the currently playing song using the Spotify Web API. By following these instructions, you will be able to integrate this feature into your applications or personal projects. So let's get started!

---

## **Step 1: Create a Spotify Developer Account**

To begin, you need to have a Spotify Developer account. Visit **[developer.spotify.com](https://developer.spotify.com/)** and log in or create a new account if you don't have one.

## **Step 2: Create a Spotify Application**

After logging in to your Spotify Developer account, create a new application. Provide the necessary details, including a name and a description. Set the **Redirect URIs** as **`http://localhost:3000`** to enable authentication.

## **Step 3: Authenticate Your Account**

To access the currently playing song, you need to authenticate your Spotify account. Follow these sub-steps:

1. Construct the authorization URL using the following link, replacing **`CLIENT_ID_HERE`** with your application's client ID: **`https://accounts.spotify.com/authorize?client_id=CLIENT_ID_HERE&response_type=code&redirect_uri=http%3A%2F%2Flocalhost:3000&scope=user-read-currently-playing`**.
2. Open the constructed URL in your browser.
3. You will be redirected to a page where you can grant access to your Spotify account.
4. After granting access, you will be redirected to the **`Redirect URI`** provided earlier (**`http://localhost:3000`**). Note the URL, as it will contain a **`code`** parameter.

## **Step 4: Generate Base64 Encoded Credentials**

To proceed, you need to generate a Base64-encoded string of your client ID and client secret. You can use an online Base64 encoder for this task. Concatenate your client ID and client secret with a colon (**`:`**) in between, and encode the resulting string.

## **Step 5: Obtain Refresh Token**

Now it's time to obtain a refresh token, which has an infinite expiry period. Follow these steps:

1. Open a terminal or command prompt.
2. Execute the following cURL request, replacing **`BASE_64_ENCODED_CODE`** with the Base64-encoded value obtained in the previous step, and **`YOUR_AUTH_CODE`** with the code you noted earlier:

```bash
curl -H "Authorization: Basic BASE_64_ENCODED_CODE" -d grant_type=authorization_code -d code=YOUR_AUTH_CODE -d redirect_uri=http%3A%2F%2Flocalhost:3000 https://accounts.spotify.com/api/token

```

1. The response will include an access token, refresh token, and other details. Make note of the refresh token.

## **Step 6: Generate Access Token**

To generate an access token using the refresh token, you can use the provided code snippet:

```jsx
const fetch = require('node-fetch');
const querystring = require('querystring');

const TOKEN_ENDPOINT = 'https://accounts.spotify.com/api/token';
const basic = Buffer.from(`${client_id}:${client_secret}`).toString('base64');

const getAccessToken = async () => {
  const response = await fetch(TOKEN_ENDPOINT, {
    method: 'POST',
    headers: {
      Authorization: `Basic ${basic}`,
      'Content-Type': 'application/x-www-form-urlencoded',
    },
    body: querystring.stringify({
      grant_type: 'refresh_token',
      refresh_token,
    }),
  });

  return response.json();
};
```

## **Step 7: Get Currently Playing Song**

Now, let's retrieve the information about the currently playing song using the provided code:

```jsx
const fetch = require('node-fetch');

const NOW_PLAYING_ENDPOINT = 'https://api.spotify.com/v1/me/player/currently-playing';

const getNowPlaying = async () => {
  const { access_token } = await getAccessToken();

  return fetch(NOW_PLAYING_ENDPOINT, {
    headers: {
      Authorization: `Bearer ${access_token}`,
    },
  });
};

const response = await getNowPlaying();
const data = await response.json();

// The `data` variable contains information about the currently playing song.
```

---

Conclusion:
Congratulations! You have successfully learned how to retrieve information
