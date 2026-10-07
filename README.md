# YouTube Channel Scraper: Subscribers, Videos, No API

Scrape YouTube channel metadata and video lists without a YouTube Data API key: subscriber count, video count, description, country, avatar, banner URL, and a list of videos with title, views, publish date, duration and thumbnail. HTTP only. No browser, no quota.

**Run it on Apify:** [apify.com/themineworks/youtube-channel](https://apify.com/themineworks/youtube-channel)
**Docs, FAQ and pricing:** [themineworks.com/actors/youtube-channel](https://themineworks.com/actors/youtube-channel/)

**Price:** From $1.80 per 1,000 channels on Apify's higher plans ($3.00 on the free plan), plus a $0.005 start fee per run. Failed and empty results are never charged.

## What it returns

* Subscriber count, video count, description and country
* Avatar and banner image URLs
* Video list: title, views, publish date, duration and thumbnail
* No YouTube Data API key required
* HTTP-only. No browser overhead, fast and cheap
* Accepts channel handles, channel IDs and @username format

## Quick start

You need a free [Apify account](https://console.apify.com/sign-up) and its API token (Settings, API & Integrations).

### Python

```bash
pip install apify-client
```

```python
from apify_client import ApifyClient

client = ApifyClient("YOUR_APIFY_TOKEN")
run = client.actor("themineworks/youtube-channel").call(run_input={
    "channelUrls": [
        "https://www.youtube.com/@MrBeast"
    ],
    "maxVideos": 5,
    "includeVideos": true
})

for item in client.dataset(run["defaultDatasetId"]).iterate_items():
    print(item)
```

### Node.js

```bash
npm install apify-client
```

```javascript
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: 'YOUR_APIFY_TOKEN' });
const run = await client.actor('themineworks/youtube-channel').call({
    "channelUrls": [
        "https://www.youtube.com/@MrBeast"
    ],
    "maxVideos": 5,
    "includeVideos": true
});
const { items } = await client.dataset(run.defaultDatasetId).listItems();
console.log(items);
```

### cURL

One request that runs the actor and returns the results in the response (for runs under 5 minutes):

```bash
curl -X POST "https://api.apify.com/v2/acts/themineworks~youtube-channel/run-sync-get-dataset-items?token=YOUR_APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"channelUrls": ["https://www.youtube.com/@MrBeast"], "maxVideos": 5, "includeVideos": true}'
```

### Command line

This repo includes ready-made clients that save results to JSON and CSV:

```bash
python3 youtube_channel_scraper.py --token YOUR_APIFY_TOKEN --channel-urls "https://www.youtube.com/@MrBeast" --max-videos "5" --include-videos
node youtube_channel_scraper.mjs --token YOUR_APIFY_TOKEN --channel-urls "https://www.youtube.com/@MrBeast" --max-videos "5" --include-videos
```

## Input

| Field | Type | Default | Description |
|---|---|---|---|
| `channelUrls` | array |  | YouTube channel URLs, handles (@name) or channel IDs |
| `maxVideos` | integer | `30` | Maximum number of videos to return per channel (from the Videos tab) |
| `includeVideos` | boolean | `true` | Fetch the channel Videos tab and include video metadata in output |

## Output

One row per result, as JSON, CSV, Excel or through the API.

| Field | Type | Description |
|---|---|---|
| `channelId` | string | YouTube channel ID (UCxxxxx...) |
| `channelHandle` | string | Channel handle (for example @MrBeast) |
| `channelName` | string | Channel display name |
| `subscriberCountText` | string | Subscriber count as shown (for example 200M subscribers) |
| `subscriberCount` | number | Subscriber count parsed to number (best-effort) |
| `videoCount` | number | Total number of videos on channel |
| `description` | string | Channel description / about text |
| `country` | string | Country of the channel |
| `channelUrl` | string | Canonical YouTube channel URL |
| `avatarUrl` | string | Channel avatar/profile image URL |
| `bannerUrl` | string | Channel banner image URL |
| `videos` | array | List of video objects from the Videos tab |
| `videoCount_fetched` | number | Number of videos actually fetched (may be less than total) |
| `status` | string | ok / error / not-found |
| `error` | string | Error message if status is error |
| `charged` | boolean | True if charged, false if free tier |
| `scrapedAt` | string | ISO-8601 timestamp |

## Use it from an AI agent

The actor works as a tool in Claude, Cursor or any MCP client through Apify's MCP server:

```
https://mcp.apify.com/?tools=themineworks/youtube-channel
```

## FAQ

### Do I need a YouTube Data API key?

No. The actor extracts channel data from the public YouTube channel page using the same ytInitialData payload the browser already loads. No API key, no quota consumption, no Google Cloud project required.

### What input formats does it accept?

Channel handles (for example @MrBeast), channel IDs (UC... format) and full channel URLs are all accepted. The actor normalises them internally.

### Does it include YouTube Shorts in the video list?

Yes. Shorts appear in the video list alongside regular uploads. The duration field distinguishes them. Shorts are always under 60 seconds.

### How accurate is the subscriber count?

YouTube displays rounded subscriber counts publicly (for example "505M" rather than the exact figure). The actor records the displayed value. For channels under 1,000 subscribers the exact number is shown.

### Can I scrape private or unlisted channels?

No. The actor only accesses publicly visible channel pages. Private channels return no data; unlisted videos do not appear in the channel video list.

### How much does the YouTube Channel Scraper cost?

From $1.80 per 1,000 channels on Apify's higher plans ($3.00 on the free plan), plus a $0.005 start fee per run. Failed and empty results are never charged. You can cap what a single run may spend with the maximum cost setting on Apify.

### Can I export the results to CSV or Excel?

Yes. Every run saves to an Apify dataset you can download as JSON, CSV, Excel or XML, or read through the API. The Python and Node clients in this repo also write the results to local files.

### Can I run it on a schedule?

Yes. Save your input as a task on Apify and attach a schedule, or call the API from your own cron job. Scheduled runs are billed the same way as manual ones.

## Related scrapers

* [Threads Scraper](https://themineworks.com/actors/threads-scraper/): Meta Threads data that returns actual data
* [Reddit Scraper](https://themineworks.com/actors/reddit-scraper/): Free Reddit data with full comment trees
* [LinkedIn Post Scraper](https://themineworks.com/actors/linkedin-post-search/): Search LinkedIn posts by keyword without login

Part of [The Mine Works](https://themineworks.com/): 151 pay-per-result scrapers with no login and no browser setup on your side.

## License

MIT © The Mine Works
