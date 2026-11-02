# Individual reflection – Lanka Explorer

I built Lanka Explorer on my own, so I did all four roles: content research for the ten destinations,
the JSON data file, the layout and CSS, and the JavaScript. My favourite part was Week 03, when the
hard-coded cards disappeared and the same `createCard()` function started building every card from
`destinations.json` – after that, searching and filtering were almost free.

The hardest problem was making the details page fast and stable. At first the page jumped badly while
the JavaScript-rendered image and the live weather were loading, and the map pushed everything down,
so the Layout Shift score was poor. I fixed it by giving the destination article and the weather box a
reserved height, loading the card images lazily but the main photo with high priority, and making the
`hidden` attribute override the button display rule so the "Add to My Trip" button really disappears
on a bad id.

If I had more time I would add a real booking flow with dates, move the form validation to a server as
well, and store the trip in the URL so a plan could be shared with a friend.
