# revmax-media

A public folder that exists for one reason: **Instagram fetches a video from a URL. It will not take
an uploaded file.**

Everything else in the RevMax stack hands over bytes — LinkedIn takes an uploaded image, Notion
takes an uploaded PNG. Instagram's publishing API is the odd one out: you give it `video_url`, and
Meta's servers go and download the file themselves. So a finished Reel sitting in
`C:\ShortsStudio\projects\<name>\WATCH THESE\` cannot be posted. It has to be somewhere public
first. This is that somewhere.

## What is in here

```
reels/<slug>.mp4    one finished vertical video per file
```

Nothing else. No code, no credentials, no build.

## Who writes to it

`npm run ig:queue` in the `linkedin-autoposter` repo. It validates the clip against Instagram's
limits, copies it in here, commits, pushes, and then **waits for the URL to answer 200** before it
writes anything to Notion — a queue row pointing at a video that is not there yet looks completely
normal and fails two days later at 9am.

## Why the files are public

They have to be, or Meta cannot reach them. The URLs are not listed or linked anywhere, but treat
anything in `reels/` as public from the moment it is pushed — which is a few days before the Reel
itself goes live. Nothing belongs in here that is not about to be published to the world anyway.

## Serving

GitHub Pages, main branch, root. Files are served at:

```
https://yousuf-creation.github.io/revmax-media/reels/<slug>.mp4
```

Pages is used rather than `raw.githubusercontent.com` because Pages returns a proper `video/mp4`
content type. Meta refuses a file served as `application/octet-stream`.

## Housekeeping

A file can be deleted once its Reel is live and the post is not going to be re-published. Git keeps
the blob in history either way, so deleting is about keeping the working tree readable, not about
reclaiming space. GitHub's hard limit is 100MB per file — which is also Instagram's limit for a
Reel, so anything that fits one fits the other.
