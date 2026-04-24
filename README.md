# kidsnews-v2

Deploy target for **News Oh, Ye!** → [news.6ray.com](https://news.6ray.com).

This repo is **not** authored by hand. The content under `site/` is produced
by a GitHub Action that pulls the latest daily zip from Supabase Storage and
commits the unpacked site. The authoring repo is
[news-v2](https://github.com/daijiong1977/news-v2) (pipeline + JSX source).

## Flow

```
news-v2 pipeline (cron)                Supabase Storage
  ├─ generates 9 articles              ┌──────────────────────┐
  ├─ writes DB rows        ──────▶     │ redesign-daily-content/
  ├─ packs website/ → zip              │    YYYY-MM-DD.zip    │
  └─ uploads zip                       │    latest.zip        │
                                       └────────────┬─────────┘
                                                    │
                                  GitHub Action pulls `latest.zip`
                                                    │
                                                    ▼
                                           kidsnews-v2 repo
                                             └ site/ (committed)
                                                    │
                                            Vercel auto-deploys
                                                    │
                                                    ▼
                                             news.6ray.com
```

## Manual re-deploy

```
gh workflow run sync-from-supabase.yml
```
