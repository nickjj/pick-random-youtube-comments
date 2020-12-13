# What is pick-random-youtube-comments? ![CI](https://github.com/nickjj/pick-random-youtube-comments/workflows/CI/badge.svg?branch=master)

It's a zero dependency Python 3 script that gets a list of top level comments
from a YouTube video and then picks N amount of unique comment authors by
choosing them randomly.

The reason I developed this script was to pick winners in a [10k subscribers
YouTube contest](https://www.youtube.com/watch?v=kYL5Jmi6slo). I asked folks to
comment in the video if they wanted to win and then this tool picked random
winners based on who commented.

## Table of contents

- [Installation](#installation)
- [Usage](#usage)
- [FAQ](#faq)
  - [What about API rate limits?](#what-about-api-rate-limits)
  - [This tool's comment count doesn't match YouTube's count](#this-tools-comment-count-doesnt-match-youtubes-count)
- [About the Author](#about-the-author)

## Installation

Since this tool has no dependencies besides Python 3.6+ you can curl it down on
your system path and run it:

```sh
sudo curl \
  -L https://raw.githubusercontent.com/nickjj/pick-random-youtube-comments/0.2.1/pick-random-youtube-comments \
  -o /usr/local/bin/pick-random-youtube-comments && sudo chmod +x /usr/local/bin/pick-random-youtube-comments
```

### Creating a Google API key

In order to access YouTube's API you will need to create an API key. It's free
and takes about 2 minutes. Here's the steps:

1. Go-to <https://console.developers.google.com/apis/credentials>
2. Create credentials with an "API Key" type
3. `export YOUTUBE_API_KEY=<your key goes here>`
4. Now you can run this tool from the same terminal

## Usage

```
pick-random-youtube-comments VIDEO_ID [--winners 10] [--omit-authors ""] [--verbose]
```

- `VIDEO_ID` The 11 characters after `?v=` in a YouTube URL, for
example `kYL5Jmi6slo`
- `--winners 10` *(optional)* Number of winners to pick (defaults to 10)
- `--omit-authors ""` *(optional)* A comma separated list of author display names to omit (defaults to nothing)
- `--verbose` *(optional)* Output the comment author display names for each page of results

Here's an example usage and output from a recent contest I ran:


```
$ pick-random-youtube-comments kYL5Jmi6slo --omit-authors "Nick Janetakis,Warren Henning"

Getting comments for page 1...

85 top level comments were returned
- 1 comment(s) had duplicate authors
- 2 comment authors were explicitly omit
= 82 comment authors have a chance to win

Winners (10):

Louis Tabosa
Dave Dyer
ouais ok
Saad Eddin
Vishal Hosamani
Joey van der wal
Ryan Markoff
Mukul Rawat
Brandon McAllister
northshorepx
```

Comments are only counted once, meaning if the same person comments 10 times
they will only have 1 chance of winning. That's what the duplicate authors
number relates to. The list of comment authors is made unique.

## FAQ

### What about API rate limits?

YouTube gives you 10,000 API credits per day. This tool uses 1 credit for each
API call it makes.

Each paginated result counts as an API call. This tool will grab 100 comments
per call which is the maximum page size.

Long story short, unless you have over a million comments for a specific video
you won't hit the rate limit. That's why this tool doesn't have an elaborate
retry mechanism since it's a use case that's not going to come up.

You can check your API quotes at
<https://console.cloud.google.com/iam-admin/quotas>.

### This tool's comment count doesn't match YouTube's count

YouTube's comment count will include both top level comments and replies to
specific comments. This tool only counts top level comments.

This was done on purpose because typically if you were to ask folks to "comment
down below to enter the contest" they would be making top level comments on
the video itself, not replying to a specific comment.

If you wanted to manually calculate the correctness of this tool you could take
this tool's "top level comments" amount (85 in usage example above), manually
count up the number of comments in any replies on your video and if you add
both values together it should match YouTube's comment count.

When I ran my contest I manually calculated everything to double check it and
it all lined up.

## About the author

- Nick Janetakis | <https://nickjanetakis.com> | [@nickjanetakis](https://twitter.com/nickjanetakis)

I'm a self taught developer and have been freelancing for the last ~20 years.
You can read about everything I've learned along the way on my site at
[https://nickjanetakis.com](https://nickjanetakis.com/). There's hundreds of
[blog posts](https://nickjanetakis.com/blog/) and a couple of [video
courses](https://nickjanetakis.com/courses/) on web development and deployment
topics. I also have a [podcast](https://runninginproduction.com) where I talk
to folks about running web apps in production.
