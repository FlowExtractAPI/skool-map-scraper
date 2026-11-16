Export the members of any Skool community you belong to, straight from the community's **member map**: each member's **exact map location and country**, plus name, bio, emails in the bio, LinkedIn, Instagram, YouTube, website, role, level, points and activity dates.

## What does Skool Members Scraper do?

Most Skool member scrapers only return the free-text "location" people type on their profile, and most members leave it empty. In a real test community, **18%** of members had typed a location, while **99.9%** had a country from the pin they placed on the map. This actor reads those pins, turns each one into a country, and lets you **filter by country before anything is extracted**. "Every member of my community in Canada and the UK" is one run.

- 📍 **Map location for every member**: latitude, longitude, country code and country name
- 🌍 **Country filter**: keep only the countries you need; members outside them are never fetched or charged
- 👤 **Full profile**: name, username, profile link, bio, time zone, avatar
- 📧 **Contact details**: emails written in the bio, plus website, LinkedIn, Instagram, YouTube, X and Facebook
- 🏆 **Community standing**: role, level, points, number of posts, join date, last active date
- ⚡ **Fast**: thousands of members per minute, so even a 20,000-member map takes minutes
- 🧩 **4 inputs, nothing to tune**: paste community links and your cookie, then press Start

### Who uses Skool map member data?

| Who | What for |
|---|---|
| **Community owners** | See where members live, plan meetups in the right cities, schedule calls in the right time zones |
| **Event organizers** | Find every member near a venue before announcing an in-person event |
| **Agencies & marketers** | Build country-specific prospect lists from communities in your niche, with LinkedIn and website links |
| **Sales teams** | Enrich your CRM with the Skool members in your territory |
| **Researchers** | Study how online communities spread across countries |

## How to scrape Skool members by location

1. **Paste the community links** into *Skool community URLs*. Any of these work: `https://www.skool.com/community-name`, `https://www.skool.com/community-name/-/map`, or just `community-name`.
2. **Paste your Skool login cookie** into *Skool login cookie* (see below; it takes a minute).
3. Optionally set **Max members per community** and pick **countries**.
4. Press **Start**, then download the results as **CSV, Excel, JSON** or fetch them through the **API**.

Your Skool account must be a **member of each community** you scrape. Skool shows the member map only to its members.

### How to get your Skool login cookie

1. Install the [Cookie-Editor](https://cookie-editor.com) browser extension.
2. Log in to [skool.com](https://www.skool.com).
3. Open Cookie-Editor, find the cookie named **`auth_token`** and copy its **value**.
4. Paste it into *Skool login cookie*.

You can also paste Cookie-Editor's whole **Export → JSON**; the actor picks `auth_token` out of it. The cookie stays valid for about a year. If you log out of Skool in your browser, copy a fresh one.

🔒 The cookie is a secret input: stored encrypted, never shown in the log or the results.

## How much does it cost to scrape Skool members?

You pay **per member delivered**:

| Plan | Price per 1,000 members |
|---|---|
| Free | $5.00 |
| Bronze | $3.00 |
| Silver | $2.00 |
| Gold and above | $1.50 |

Members outside your country filter are skipped **before** their profile is fetched, so you never pay for them, and rows explaining why a community returned nothing are never charged. Set a spending limit on the run and the actor stops exactly there, without fetching profiles it can't deliver.

## Input and output examples

### Input

| Field | What it does |
|---|---|
| 🔗 **Skool community URLs** | Communities to extract: community link, map link or plain community name |
| 🍪 **Skool login cookie** | Your `auth_token` cookie value (or the Cookie-Editor JSON export) |
| 🔢 **Max members per community** | Stop after this many members in each community. Empty or `0` = every member on the map |
| 🌍 **Only members in these countries** | Keep only members whose map pin is in these countries. Empty = every country |

```json
{
    "communityUrls": ["https://www.skool.com/your-community", "another-community"],
    "skoolCookie": "<your auth_token value>",
    "maxMembers": 500,
    "countries": ["US", "CA", "GB"]
}
```

With a country filter, **Max members** counts only members in those countries: `maxMembers: 100` with `countries: ["CA"]` returns up to 100 Canadians.

### Output

One row per member pinned on the map (sample data):

```json
{
    "community": "your-community",
    "communityName": "Your Community",
    "userName": "jane-doe",
    "fullName": "Jane Doe",
    "profileUrl": "https://www.skool.com/@jane-doe",
    "latitude": 45.5019,
    "longitude": -73.5674,
    "countryCode": "CA",
    "country": "Canada",
    "location": "Montreal",
    "timeZone": "America/Toronto",
    "bio": "Helping coaches automate their business. Write me: jane@example.com",
    "bioEmails": ["jane@example.com"],
    "linkWebsite": "https://janedoe.example",
    "linkLinkedIn": "https://www.linkedin.com/in/jane-doe",
    "linkInstagram": "https://www.instagram.com/janedoe",
    "linkYoutube": null,
    "role": "member",
    "level": 5,
    "points": 1240,
    "postsCount": 37,
    "joinedAt": "2025-03-02T14:11:08Z",
    "lastActiveAt": "2026-11-15T09:42:51Z",
    "accountCreatedAt": "2024-12-20T18:03:40Z",
    "totalGroupsMemberOf": 6,
    "totalGroupsCreated": 1,
    "avatarUrl": "https://assets.skool.com/f/.../avatar.jpg",
    "source_url": "https://www.skool.com/your-community/-/map",
    "extractedAt": "2026-11-16T16:00:00.000Z"
}
```

Fields with no value are `null`, so every row has the same columns in CSV and Excel. The dataset has three ready-made views: **Members on the map**, **Contacts & socials** and **All fields**.

### When a community returns no members

A run never ends empty without a reason. A community that can't be extracted gives one uncharged row with `notice` and `message`:

| `notice` | Meaning | What to do |
|---|---|---|
| `COOKIE_MISSING` | No login cookie was given | Add your `auth_token` cookie |
| `COOKIE_EXPIRED` | Skool did not accept the cookie | Copy a fresh `auth_token` and run again |
| `NOT_A_MEMBER` | Your account hasn't joined this community | Join it on Skool, then run again |
| `PENDING_APPROVAL` / `DECLINED` | Your join request is waiting, or was declined | Wait for the admin, or use another account |
| `MAP_DISABLED` | The community has its map turned off | Nothing to extract |
| `MAP_EMPTY` | Nobody has pinned a location yet | Nothing to extract |
| `NO_MATCH` | No member is pinned in the countries you picked | Widen the country filter |
| `NOT_FOUND` / `INVALID_URL` | The link isn't a Skool community | Check the URL |

## More Skool tools

Need the content behind the members? **[Skool Scraper Pro](https://apify.com/dz_omar/skool-scraper-pro?fpr=smcx63)** extracts courses, lessons, videos, posts and comments, and finds new communities by category or keyword.

## FAQ

### Do I need to be a member of the community?
Yes. Skool shows the member map only to members, and the actor sees exactly what you see when you open the map in your browser.

### Does it work for paid and private communities?
Yes, as long as your account is an approved member.

### Why are fewer members exported than the community size?
The map only contains members who chose to pin their location. In a 28,000-member community we tested, 23,000 had a pin.

### Can I export every member of a community with 100,000+ members?
Yes. Leave *Max members per community* empty. If the run is interrupted, it resumes where it stopped.

### How accurate is the country?
It comes from the pin the member placed on the map. Pins dropped in the sea keep their coordinates and get `null` for the country.

### Can I scrape several communities at once?
Yes. Add as many links as you like; duplicates are removed automatically.

### Can I schedule it or use it from code?
Yes. Run it on a schedule, through the Apify API, or connect it to Make, Zapier, n8n or Google Sheets.

### Something doesn't work. Where do I get help?
Open an issue on the actor's **Issues** tab or email us (below). Custom fields and integrations are welcome.

## Support

- 🌐 **Website:** [flowextractapi.com](https://flowextractapi.com)
- 📧 **Email:** [flowextractapi@outlook.com](mailto:flowextractapi@outlook.com)
- 💬 **GitHub:** [FlowExtractAPI](https://github.com/FlowExtractAPI)
- 💼 **LinkedIn:** [flowextract-api](https://www.linkedin.com/in/flowextract-api/)
- 🐦 **X (Twitter):** [@FlowExtractAPI](https://x.com/FlowExtractAPI)
- 📱 **Facebook:** [flowextractapi](https://www.facebook.com/flowextractapi)
- 🎵 **TikTok:** [@flowextractapi](https://www.tiktok.com/@flowextractapi)

## Legal & compliance

- **Member-visible data only**: extracts only what a logged-in member already sees on the community map and member profiles.
- **Rate limiting**: respects source-service limits and terms of use.
- **Data protection**: no storage of personal information beyond your own run's results. Member data is personal data, so comply with GDPR, CCPA and the laws that apply to you, and don't use it for spam.
- **Commercial use**: suitable for business intelligence, events and research.
- No affiliation with or endorsement by Skool is implied.

---

*Skool Members Scraper by FlowExtract API. Turn any website into structured data.*
