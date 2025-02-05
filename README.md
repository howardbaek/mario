
[![Travis build
status](https://travis-ci.com/jhudsl/mario.svg?branch=master)](https://travis-ci.com/jhudsl/mario)
[![AppVeyor Build
Status](https://ci.appveyor.com/api/projects/status/github/jhudsl/mario?branch=master&svg=true)](https://ci.appveyor.com/project/jhudsl/mario)
<!-- README.md is generated from README.Rmd. Please edit that file -->

# mario Package:

The goal of `mario` is to automatically create videos from a set of
Google Slides. Whatever is written in the speaker notes section of the
Google Slides will be read in the video.

If you update the slides, all you need to do is re-run `mario` to update
the video.

## Installation

You can install `mario` from GitHub with:

``` r
# install.packages("remotes")
remotes::install_github("jhudsl/mario")
```

## Example

Before you can run `mario`, you will need two things:

1)  [API Key](#api-key)
2)  [Google Slides ID](#google-slides-id)

### API Key

You will need access to the mario RSConnect through your institution.
For example, JHU users will login at
<https://rsconnect.biostat.jhsph.edu/connect/>. Once there, you’ll need
to obtain an API token. Click on your profile in the upper right corner
\> `API Keys` \> `+ New API Key` and copy that API key token.

In your local RStudio, copy your API key in a command like this and run:

``` r
Sys.setenv(CONNECT_API_KEY = "your-api-key")
```

It is stored in the environment for the current R session. You should
only need to do this once per your RStudio environment. Your API key is
called `CONNECT_API_KEY`. You can call it something else, but this is
the name that the API key functions below use by default, e.g.:

``` r
mario_auth(api_key = Sys.getenv("CONNECT_API_KEY"))
```

Now to test if your API key has been set up correctly, run this:

``` r
library(mario)

if (mario_have_api_key()) {
  mario_api_key()
}
```

If set up correctly, it should repeat back to you your API key.

### Google Slides ID

If you have a Google Slides set, you can obtain the Google Slides set ID
from the URL:
`https://docs.google.com/presentation/d/**presentationId**/edit`

Your [Google Slides
permissions](https://www.youtube.com/watch?v=zHSfwaZIVbM) must be set to
`Anyone with the link`. For testing purposes, we’ve included a set of
[test
slides](https://docs.google.com/presentation/d/1sFsRXfK7LKxFm-ydib5dxyQU9fYujb95katPRu0WVZk/edit#slide=id.p)
you can use to practice.

## Running Mario

``` r
# Google Slides ID
id <- "1sFsRXfK7LKxFm-ydib5dxyQU9fYujb95katPRu0WVZk"

# Run mario!
res <- mario::mario(id,
  voice = "en-US-Wavenet-F")

# Write the video
mario::mario_write_video(
  res)
```

Mario will print a file path to the newly rendered video in the console.
If you’d like to see a list of all the voice options:

``` r
voice_options <- mario_voices()
head(voice_options)
```

## Getting Mario subtitles for YouTube

``` r
# Extract the subtitles
subtitles <- mario::mario_subtitles(res)
# Write the subtitles to file
readr::write_lines(subtitles, "out.txt")
```

On YouTube:

- Navigate to <https://studio.youtube.com>
- Go to “Subtitles” on the left hand menu
- Click on the video to which you want to add subtitles
- Click the “ADD” link under “Subtitles” in the table
- Select Upload File and select “With timing”
- Upload the “out.txt” file created above
- Once checked, click “PUBLISH”
