# Twitter Timeline Data

Pre-filtered Twitter timeline candidates, updated every 30 minutes.

## Data

Daily JSON files in `data/` directory: `candidates_YYYY-MM-DD.json`

Each file is append-only throughout the day. New tweets are added every ~30 minutes.

## Schema

Each tweet object:

| Field | Type | Description |
|-------|------|-------------|
| `id` | string | Tweet ID |
| `text` | string | Tweet text |
| `author` | string | Author handle |
| `author_name` | string | Author display name |
| `author_followers` | int | Follower count at fetch time |
| `is_rt` | bool | Is retweet |
| `rt_by` | string? | Retweeted by (if RT) |
| `is_quote` | bool | Is quote tweet |
| `quoted_text` | string? | Quoted tweet text |
| `quoted_author` | string? | Quoted tweet author |
| `retweet_count` | int | Retweet count |
| `like_count` | int | Like count |
| `reply_count` | int | Reply count |
| `created_at` | string | Tweet creation time (Twitter format) |
| `urls` | array | Extracted URLs |
| `lang` | string | Language code |
| `_source` | string | `following` or `foryou` |
| `_fetched_at` | string | ISO 8601 fetch timestamp (ET) |

## Digest

AI-curated digest distilled from that day's candidates, refreshed every 4 hours.
Daily JSON files in `digest/` directory: `digest_YYYY-MM-DD.json`. Each file is
overwritten in place as the day progresses (latest snapshot wins), retained for
7 days.

Top-level object:

| Field | Type | Description |
|-------|------|-------------|
| `topics` | array | Chinese one-line summaries of the selected items |
| `items` | array | Selected items (see below) |
| `last_updated` | string | Date of the latest refresh (YYYY-MM-DD) |

Each item object:

| Field | Type | Description |
|-------|------|-------------|
| `category` | string | `🔴 立即关注` / `🟡 投资机会 & 策略` / `🟠 Alpha 信号` / `🔵 值得一读` |
| `summary` | string | Chinese summary of the tweet's claim |
| `author` | string | Author handle (no `@`) |
| `id` | string | Source tweet ID (joins back to `data/candidates_*.json`) |

## Pre-filter Criteria

Tweets pass if any of these conditions are met:
- High engagement (likes ≥ 500 or retweets ≥ 100)
- Large account (followers ≥ 500K)
- Known institutional/notable account
- Contains relevant keywords (regulatory, security, funding, AI, crypto, DeFi, strategy)
- Moderate engagement (likes ≥ 50) with token ticker mentions
- Buzz detection (2+ authors mentioning same handle/ticker)

## Update Frequency

- New data appended every ~30 minutes
- Pushed to GitHub after each update
- Each day produces ~4,000-5,000 tweets
- File size: ~3-5 MB per day
- Files retained for 7 days

## License

This data consists of publicly available Twitter content. For research purposes only.
