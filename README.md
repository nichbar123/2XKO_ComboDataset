# 2XKO Combo Dataset -- Backend Extraction & Cleaning

## Overview

This project builds a structured dataset of community-submitted **2XKO
combos** sourced directly from the live production database powering
[-](2xkombo.gg).

Instead of scraping rendered HTML, this project extracts data from the
site's Supabase REST API endpoint, ensuring:

-   Structured relational data\
-   Clean JSON responses\
-   No DOM scraping\
-   Reproducible pipeline\
-   Ethical, minimal-load extraction

The dataset is intended for academic use, including data cleaning,
sequence analysis, and modeling.

------------------------------------------------------------------------

## Data Source

Data was retrieved from the public Supabase REST endpoint used by the
frontend application:

https://ifumpbjbjignkolkjxtt.supabase.co/rest/v1/combos

Query parameters:

select=\*,profiles!inner(username)\
approved=eq.true

Only approved combos were included.

------------------------------------------------------------------------

## Extraction Method

The dataset was retrieved using Python and the public Supabase `anon`
API key exposed in frontend requests.

Example extraction script:

``` python
import requests
import pandas as pd

ANON_KEY = "YOUR_ANON_KEY"

url = "https://ifumpbjbjignkolkjxtt.supabase.co/rest/v1/combos"

params = {
    "select": "*,profiles!inner(username)",
    "approved": "eq.true"
}

headers = {
    "apikey": ANON_KEY,
    "Authorization": f"Bearer {ANON_KEY}"
}

r = requests.get(url, headers=headers, params=params)
data = r.json()

df = pd.DataFrame(data)
df.to_csv("2xko_raw_combos.csv", index=False)
```

------------------------------------------------------------------------

## Dataset Structure

The dataset includes 134 approved combos (at time of extraction).

Key columns:

-   id\
-   created_at\
-   character_1\
-   character_2\
-   title\
-   description\
-   notation\
-   damage\
-   meter_cost\
-   fuse_type\
-   fury\
-   game_version\
-   upvotes\
-   downvotes\
-   profiles.username

------------------------------------------------------------------------

## Cleaning Workflow (OpenRefine)

Typical cleaning steps:

1.  Flatten nested `profiles.username`
2.  Trim whitespace in `notation`
3.  Replace `->` with spaces
4.  Remove commas
5.  Collapse multiple spaces
6.  Split notation into token sequences
7.  Create derived fields such as combo length and damage per hit

------------------------------------------------------------------------

## Potential Analytical Applications

-   Sequence modeling (Markov chains)
-   N-gram analysis
-   Damage regression modeling
-   Meter efficiency analysis
-   Combo length distribution analysis
-   Character pairing analysis
-   Clustering combo archetypes
-   Text mining on descriptions

------------------------------------------------------------------------

## Ethical Considerations

-   No HTML scraping was performed.
-   Data was retrieved from publicly accessible API endpoints used by
    the site itself.
-   Only approved, public records were included.
-   No private user information was accessed.

------------------------------------------------------------------------

## License

For academic and research use only.

