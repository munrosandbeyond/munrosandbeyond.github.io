# Munros & Beyond - Blog Maintenance Guide

## Tech Stack
- Jekyll static site on GitHub Pages, using the minima theme with heavy CSS overrides
- Leaflet.js for interactive maps
- Google Fonts: Inter + Lora

## Adding a New Blog Post

1. Create a file in `_posts/` named `YYYY-MM-DD-slug.md`
2. Required front matter:
```yaml
---
layout: post
title: "Post Title"
description: "Short description for cards"
date: YYYY-MM-DD          # publish date (affects URL)
trip_date: "Month YYYY"   # display date (e.g. "June 2025")
author: Dewi Gould
image: /assets/your-hero-image.JPG
category: scotland         # or "further-afield"
thumbnail: "/assets/your-thumbnail.JPG"
strava_id: 12345678        # optional, for auto-embed
location_lat: 56.83        # optional, for World Map pin
location_lng: -4.97
---
```
3. The `category` field affects the URL: `category: scotland` generates `/scotland/YYYY/MM/DD/slug.html`
4. Posts appear on the homepage (all posts), and on their category page (Scotland Trips or Further Afield)
5. If `location_lat`/`location_lng` are set, a pin appears automatically on the World Map page

## Adding a Route Map

1. Put GPX files in `assets/gpx/` with descriptive names
2. Create an include in `_includes/` (e.g. `your-route-map.html`) - see `solstice-route-map.html` or `ak-suu-route-map.html` as templates
3. The include renders a Leaflet map with the route, munro summit markers, camp/bivvy markers, and a canvas elevation profile
4. Add `{% raw %}{% include your-route-map.html %}{% endraw %}` to the blog post

## Adding a Strava Activity Embed

- Use `{% raw %}{% include strava-activity.html id="ACTIVITY_ID" %}{% endraw %}` in the post body
- Or set `strava_id` in front matter for auto-embed at the bottom
- Note: Group activities (recorded by/with others) may not embed due to Strava privacy restrictions

## Quick Facts Box

```
{% raw %}{% include quick-facts.html
  distance="48 km"
  elevation="3,345 m total"
  time="3 days"
  difficulty="Hard"
  start="Corrour Station"
  peaks="Peak 1, Peak 2"
  season="May - Sep"
%}{% endraw %}
```

## When New Munros Are Completed

You must update **all of the following** in `munros.html`:

1. **`completed` array** (~line 340-400): Add an entry for each new munro:
   ```js
   {name:"Munro Name",lat:56.xxxx,lng:-x.xxxx,date:"YYYY-MM-DD",note:"",blog:"/category/YYYY/MM/DD/slug.html"},
   ```
   - The `name` must exactly match the entry in the `allMunros` array
   - The `blog` field is optional - add it if there's a blog post for this climb
   - The `note` field should be empty `""`

2. **`stravaByDate` object** (~line 437-489): Add a date-to-activity mapping:
   ```js
   "YYYY-MM-DD": STRAVA_ACTIVITY_ID,
   ```

3. **Munro count on homepage** (`_layouts/home.html` ~line 11): Update "Munros Completed: X / 282"

4. **Munro count on about page** (`about.md`): Update the progress display (115 target arrow 282)

5. **The `allMunros` array** (~line 40-160) contains all 282 munros and should NOT need updating unless a munro is added/removed from the official list

### Cross-linking

- The munro map auto-generates clickable popups with blog links and Strava links based on the `blog` and `stravaByDate` fields
- The search feature on the munros page also uses these fields to show "Read blog" and "View on Strava" links
- The progress-over-time chart auto-updates from the completed array

## Important Notes

- **Cache busting**: CSS is loaded with `?v=2` in `_includes/head.html`. Increment this if CSS changes aren't showing
- **Image alt text**: All images in posts should have alt text for accessibility
- **Lightbox**: Post content images automatically get click-to-zoom via the lightbox in `_layouts/default.html`
- **Map tiles**: Using CARTO Voyager raster tiles - the URL must include `rastertiles/` in the path
- **Blog URL structure**: Jekyll prepends the category to the URL. A post with `category: scotland` and date `2026-03-07` and slug `summer-solstice` will be at `/scotland/2026/03/07/summer-solstice.html`

## File Structure

```
_layouts/
  default.html    - Base layout (nav, footer, lightbox)
  home.html       - Homepage (hero, post cards grid)
  post.html       - Blog post layout (hero image, content)

_includes/
  head.html       - Google Fonts, Leaflet, CSS links
  gpx-map.html    - Simple single-GPX route map
  strava-activity.html - Strava embed component
  quick-facts.html     - Info panel for trip stats
  ak-suu-route-map.html    - Kyrgyzstan 5-day route map
  solstice-route-map.html  - Solstice 3-day route map

css/
  override.css    - All custom styles (~1200 lines)

assets/gpx/       - GPX route files

_posts/           - Blog posts (markdown)

munros.html       - Interactive munro tracker (map, search, list, chart)
scotland.html     - Scotland category page
further-afield.html - Further Afield category page
about.md          - About page
```
