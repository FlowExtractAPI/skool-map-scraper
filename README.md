# 🗺️ Skool Map Scraper

Extract **member locations and full public profiles** from any Skool community map page  coordinates, names, bios, social links, levels, points, and more  organized and ready to use.

Perfect for **community owners**, **marketers**, and **researchers** who need geographic member data without manual effort.

- A short guide on how to use **[Skool Map Scraper](https://apify.com/dz_omar/skool-map-scraper?fpr=smcx63)** Actors :

[Skool Map Scraper](https://www.youtube.com/watch?v=u-i-Korzf8w)

---

## 🚀 Key Benefits & Use Cases

### **📍 For Community Owners**
- Map your members geographically to plan regional events or meetups
- Export full member profiles with social links, bios, and engagement levels
- Track community growth across regions

### **💼 For Marketers & Analysts**
- Build geo-targeted audience segments from member coordinates
- Analyze member distribution across countries and cities
- Cross-reference social profiles (LinkedIn, Instagram, Twitter) for outreach

### **🎯 For Researchers**
- Extract structured member data from any Skool community at scale
- Study community demographics by location, role, and engagement level
- Bulk-export 200,000+ member profiles with full metadata

---

## 🔍 What Gets Extracted

### 👤 **Member Identity**
- User ID, username, first name, last name, full name
- Profile URL (`https://www.skool.com/@username`)

### 🖼️ **Media**
- Full-size avatar URL
- Thumbnail avatar URL

### 📝 **Profile Data**
- Bio text
- Self-reported location (free text)
- Community role (member / admin / moderator)
- Level and points (from Skool's gamification system)

### 📍 **Geographic Coordinates**
- Latitude (WGS-84 decimal degrees)
- Longitude (WGS-84 decimal degrees)

### 📅 **Timestamps**
- Joined at, approved at, last offline, account created at

### 🔗 **Social Links**
- Facebook, Instagram, LinkedIn, Twitter/X, Website, YouTube

### 📊 **Community Stats**
- Total groups member of, total groups created, shared groups count

### 🏷️ **Metadata**
- Community slug, community ID, extraction timestamp

---

## ⚙️ Input Options

### 🔗 Community URLs

The actor accepts one or more Skool community URLs in any format:

| Input value | What it extracts |
|---|---|
| `https://www.skool.com/learn-ai` | All map members from `learn-ai` |
| `https://www.skool.com/learn-ai/-/map` | Same  `/-/map` suffix is stripped |
| `learn-ai` (bare slug) | Same  auto-prefixed with `https://www.skool.com/` |

You can mix multiple URLs to extract from several communities in one run:

```json
{
    "communityUrl": [
        "https://www.skool.com/learn-ai",
        "https://www.skool.com/ai-automation-society/-/map"
    ]
}
```

### 📊 Extraction Options

#### `maxMembersPerCommunity` (Integer)
- **Default**: `10`
-   To extract all members, just put a large number in "maxMembersPerCommunity": 1000000,.
- Maximum number of member profiles to extract from EACH community
- Set to `100` → extracts up to 100 members from each community URL
- Useful for testing before running full extractions on large communities (225,000+ members)

#### `userPreviewConcurrency` (Integer)
- **Default**: `10`
- How many user profile requests to fire simultaneously (1–20)

```json
{
    "communityUrl": ["https://www.skool.com/learn-ai"],
    "maxMembersPerCommunity": 500,
    "userPreviewConcurrency": 10
}
```

### 🔐 Authentication

You **must be a member** of the community to access its map. The actor tries authentication in priority order:

| Priority | Method | When to use |
|---|---|---|
| 1 | **Email + Password** | Easiest  just enter your Skool credentials |
| 2 | **Browser Cookies** | Fallback if login fails, or if you prefer cookie-based auth |
| 3 | **None** | Public communities only (most maps require membership) |

#### Method 1  Email + Password (recommended)

```json
{
    "email": "you@example.com",
    "password": "your-password"
}
```

#### Method 2  Browser Cookies (fallback)

1. Install the [Cookie-Editor extension](https://chrome.google.com/webstore/detail/cookie-editor/hlkenndednhfkekhgcdicdfddnkalmdm)
2. Log in to Skool, navigate to your community
3. Open Cookie-Editor → click Export → copy the JSON
4. Paste into the `cookies` field:

```json
{
    "cookies": [
        { "name": "auth_token", "value": "your-token-value", "domain": ".skool.com" }
    ]
}
```

> ⚠️ Cookies expire. If extraction fails with 403 errors, export fresh cookies and re-run.

---

## 📊 Sample Output

```json
{
    "extractedAt": "2026-03-08T19:56:19.151Z",
    "community": "learn-ai",
    "communityId": "83f66d7eb37844e2adbdab53dd48b3ce",
    "userId": "f185d42b54884f669ffb900afe6bb1f1",
    "userName": "nick-neville",
    "firstName": "Nick",
    "lastName": "Neville",
    "fullName": "Nick Neville",
    "profileUrl": "https://www.skool.com/@nick-neville",
    "avatarUrl": "https://assets.skool.com/f/.../md.jpg",
    "avatarThumbUrl": "https://assets.skool.com/f/.../sm.jpg",
    "bio": "Founder of JustCoach\nHelping sports coaches create and scale online coaching businesses.",
    "location": "Scottsdale, AZ",
    "latitude": 33.41689906,
    "longitude": -111.88929107,
    "role": "member",
    "level": 1,
    "points": 0,
    "joinedAt": "2025-02-21T07:23:44.727202Z",
    "approvedAt": "2025-02-21T07:24:57.47048Z",
    "lastOffline": "2026-03-08T19:36:55.492869Z",
    "memberCreatedAt": "2023-09-30T20:52:43.46592Z",
    "totalGroupsMemberOf": 0,
    "totalGroupsCreated": 1,
    "totalSharedGroups": 0,
    "linkFacebook": "https://www.facebook.com/therealnickneville",
    "linkInstagram": "http://www.instagram.com/therealnickneville",
    "linkLinkedIn": "https://www.linkedin.com/in/nick-neville-justcoach",
    "linkTwitter": "https://x.com/realnickneville",
    "linkWebsite": "linktr.ee/justcoach",
    "linkYoutube": "https://www.youtube.com/@nick_neville"
}
```

## 🔄 Resumability

The actor saves state after every batch of 50 users. If it crashes, gets migrated, or is aborted, it resumes exactly where it left off  no duplicate work.

| Trigger | What gets saved |
|---|---|
| After every batch of 50 users | `processedUserIds` (incremental) |
| Apify `persistState` event (~60s) | Full state snapshot |
| Apify `aborting` event | Full state snapshot |
| After each community URL completes | `processedUrls` (new URL appended) |
| End of run | Full state snapshot |

For a 100,000-member community, this guarantees at most 50 duplicate rows on a crash restart. The `isUserDone()` check on reload skips the duplicates anyway.

---

## 🌐 Proxy Support

The actor routes all HTTP requests through a residential proxy to avoid IP blocks. The proxy is selected automatically based on your Apify pricing tier:

| User tier | Proxy used | How |
|---|---|---|
| 💎 Paying |  Premium Residential | Faster, more reliable |
| 🆓 Free | Apify RESIDENTIAL | Built-in, automatic |
| ⚠️ None | Direct connection | No proxy configured |

## 🚫 Access Error Handling

The actor detects exactly why a community map is inaccessible and gives clear, actionable messages:

| Situation | What you see | What to do |
|---|---|---|
| Not a member | `🚫 ACCESS DENIED` box with join link | Visit the community's `/about` page and join |
| Membership declined | `⛔ MEMBERSHIP DECLINED` box | Contact the admin or try a different account |
| Pending approval | `⏳ PENDING APPROVAL` box | Wait for admin approval, then re-run |
| Empty map | Warning message | No members have pinned their location |

---
## Actor Standby

### 🔌 Standby Mode (Actor-to-Actor Streaming)

This actor supports **Standby mode**  it runs as a persistent HTTP server that another actor can call to stream member data in real-time via NDJSON.

### How it works

When launched in Standby mode, the actor starts an Express server and stays alive indefinitely. Another actor (the "caller") sends a POST request with community URLs, and this actor streams back member data line-by-line as it's enriched  no waiting for the full extraction to complete.

The public URL:
```
https://dz-omar--skool-map-scraper.apify.actor
```

### Endpoints

| Endpoint | Method | Purpose |
|----------|--------|---------|
| `/heartbeat` | GET | Keep-alive ping  wakes the actor if cold |
| `/` | GET | Readiness probe  returns server status |
| `/` | POST | Main endpoint  accepts communities, streams members back |

### POST / Request Body

```json
{
    "communityUrls": [
        "https://www.skool.com/learn-ai",
        "https://www.skool.com/ai-automation-society"
    ],
    "email": "your@email.com",
    "password": "your-password",
    "userPreviewConcurrency": 10,
    "maxMembersPerCommunity": 500,
    "alreadyProcessedUserIds": [],
    "startFromCommunityIndex": 0
}
```

The last two fields are for **resumption**  if the connection drops, the caller sends back the user IDs it already received and the community index to resume from.

### NDJSON Event Types

| Type | When | Key fields |
|------|------|------------|
| `log` | Informational message | `message` |
| `batch` | Batch of enriched members ready | `members`, `membersSentSoFar`, `totalInCommunity`, `completed` |
| `community_complete` | All members for one community sent | `communitySlug`, `communityUrl`, `membersSent` |
| `complete` | All communities done |  |
| `error` | Something failed | `error`, `errorType`, `isRetryable` |
| `migrating` | Apify migrating the server | `membersSentSoFar`, `alreadyProcessedUserIds` |
| `aborting` | Actor shutting down | `membersSentSoFar`, `alreadyProcessedUserIds` |

### Example: Calling from Another Actor

```javascript
const MAP_SCRAPER_URL = 'https://dz-omar--skool-map-scraper.apify.actor';
const HEARTBEAT_URL = `${MAP_SCRAPER_URL}/heartbeat`;
const MAX_RECONNECT = 3;
const RECONNECT_DELAY = 2000;

// Wake up the standby actor
async function ping() {
    try {
        await fetch(HEARTBEAT_URL, {
            headers: { 'Authorization': `Bearer ${process.env.APIFY_TOKEN}` },
        });
    } catch (_) {}
}

// Single connection attempt  reads NDJSON stream
async function fetchOnce(communityUrls, progress) {
    await ping();

    const response = await fetch(MAP_SCRAPER_URL, {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
            'Authorization': `Bearer ${process.env.APIFY_TOKEN}`,
        },
        body: JSON.stringify({
            communityUrls,
            email: 'your@email.com',
            password: 'your-password',
            maxMembersPerCommunity: 500,
            alreadyProcessedUserIds: progress.processedUserIds,
            startFromCommunityIndex: progress.communityIndex,
        }),
    });

    if (!response.ok) throw new Error(`HTTP ${response.status}`);

    const heartbeat = setInterval(ping, 30000);
    const reader = response.body.getReader();
    const decoder = new TextDecoder();
    let buffer = '';

    try {
        while (true) {
            const { done, value } = await reader.read();
            if (done) break;

            buffer += decoder.decode(value, { stream: true });
            const lines = buffer.split('\n');
            buffer = lines.pop() || '';

            for (const line of lines) {
                if (!line.trim()) continue;
                const event = JSON.parse(line);

                switch (event.type) {
                    case 'log':
                        console.log(`[MAP-SCRAPER] ${event.message}`);
                        break;

                    case 'batch':
                        for (const member of event.members) {
                            progress.allMembers.push(member);
                            progress.processedUserIds.push(member.userId);
                        }
                        console.log(`Received ${event.batchSize} members (total: ${event.membersSentSoFar})`);
                        break;

                    case 'community_complete':
                        progress.communityIndex++;
                        console.log(`Done: ${event.communitySlug} (${event.membersSent} members)`);
                        break;

                    case 'complete':
                        progress.completed = true;
                        break;

                    case 'migrating':
                    case 'aborting':
                        if (event.alreadyProcessedUserIds) {
                            progress.processedUserIds = event.alreadyProcessedUserIds;
                        }
                        throw new Error(`Server ${event.type}  will reconnect`);

                    case 'error':
                        if (event.isRetryable) throw new Error(`Retryable: ${event.error}`);
                        console.error(`Non-retryable: ${event.error}`);
                        break;
                }
            }
        }
    } finally {
        clearInterval(heartbeat);
    }
}

// Main entry point with reconnect loop
export async function scrapeMapData(communityUrls) {
    const progress = { communityIndex: 0, processedUserIds: [], allMembers: [], completed: false };
    let attempt = 0;

    while (!progress.completed && attempt < MAX_RECONNECT) {
        try {
            await fetchOnce(communityUrls, progress);
        } catch (error) {
            attempt++;
            console.error(`Attempt ${attempt}/${MAX_RECONNECT}: ${error.message}`);
            if (attempt < MAX_RECONNECT) await new Promise(r => setTimeout(r, RECONNECT_DELAY));
        }
    }

    return progress.allMembers;
}
```

---

## 🤝 Support & Resources

- 🌐 **Website**: [flowextractapi.com](https://flowextractapi.com)
- 📧 **Email**: [flowextractapi@outlook.com](mailto:flowextractapi@outlook.com)
- 🙋 **Apify Profile**: [FlowExtract API](https://apify.com/dz_omar?fpr=smcx63)
- 💬 **GitHub**: [FlowExtractAPI](https://github.com/FlowExtractAPI)

### Social Media
- 💼 **LinkedIn**: [flowextract-api](https://www.linkedin.com/in/flowextract-api/)
- 🐦 **Twitter**: [@FlowExtractAPI](https://x.com/@FlowExtractAPI)
- 📱 **Facebook**: [flowextractapi](https://www.facebook.com/flowextractapi)

---

## 🌟 Related Actors by FlowExtractAPI

### 📚 Education & Community
- **[Skool Scraper Pro](https://apify.com/dz_omar/skool-scraper-pro?fpr=smcx63)**  Lessons, videos, posts, and attachments from Skool classrooms

### 🎬 Video & Media
- **[Loom Scraper](https://apify.com/dz_omar/loom-video-scraper?fpr=smcx63)**  Loom video & transcript extraction
- **[YouTube Scraper Pro](https://apify.com/dz_omar/Youtube-Scraper-Pro?fpr=smcx63)**  Complete channel and playlist extraction
- **[Zoom Scraper](https://apify.com/dz_omar/zoom-scraper?fpr=smcx63)**  Download Zoom recordings and transcripts
- **[TikTok Video Downloader](https://apify.com/dz_omar/tiktok-video-downloader?fpr=smcx63)**  TikTok without watermarks

### 🛠️ Developer Tools
- **[Universal Downloader](https://apify.com/dz_omar/universal-downloader?fpr=smcx63)**  Download any file type with proxy support
- **[Ultimate Screenshot](https://apify.com/dz_omar/ultimate-screenshot?fpr=smcx63)**  Advanced website screenshot tool

### 📱 Social & Ads
- **[Facebook Ads Scraper Pro](https://apify.com/dz_omar/facebook-ads-scraper-pro?fpr=smcx63)**  Facebook Ad Library extraction
- **[Google Ads Scraper](https://apify.com/dz_omar/google-ads-scraper?fpr=smcx63)**  Google Ads Transparency Center data
