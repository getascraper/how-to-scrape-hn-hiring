# How to Scrape Hacker News Who is Hiring Posts in Node.js

Extract structured job postings from Hacker News monthly "Who is Hiring?" threads using the [HN Who is Hiring Scraper](https://apify.com/devanshlive/hn-hiring-scraper) actor on Apify -- no browser automation or proxies required.

## What this example does

- Calls the HN Who is Hiring Scraper actor via the Apify client
- Passes optional post URLs or auto-discovers the latest threads
- Waits for the run to complete
- Fetches results from the actor's dataset
- Prints each job posting to the console

## Prerequisites

- [Node.js](https://nodejs.org/) v18 or higher
- An [Apify account](https://console.apify.com/sign-up) (free tier available)
- An [Apify API token](https://console.apify.com/settings/integrations)

## Installation

```bash
npm install
```

## Environment setup

```bash
cp .env.example .env
```

Open `.env` and replace `your_apify_token_here` with your actual Apify API token.

## Usage

```bash
npm start
```

## Code example

```javascript
import { ApifyClient } from 'apify-client';
import 'dotenv/config';

const client = new ApifyClient({
  token: process.env.APIFY_TOKEN,
});

const input = {
  monthsBack: 1,
  maxJobsPerMonth: 10,
  includeReplies: false,
};

const run = await client.actor('devanshlive/hn-hiring-scraper').call(input);

console.log('Results from dataset');
console.log(`Check your data here: https://console.apify.com/storage/datasets/${run.defaultDatasetId}`);
const { items } = await client.dataset(run.defaultDatasetId).listItems();
items.forEach((item) => {
  console.dir(item);
});
```

## Example output

See `sample-output.json` for a full example. Each job posting includes:

| Field | Description |
|-------|-------------|
| `commentId` | Unique HN comment ID |
| `hnUser` | HN username who posted the job |
| `postedAt` | Unix timestamp of the post |
| `postedAtIso` | ISO timestamp of the post |
| `rawText` | Original comment text |
| `cleanText` | Parsed and cleaned text |
| `company` | Company name (extracted from post) |
| `role` | Job role/title |
| `location` | Job location |
| `remoteStatus` | REMOTE, ONSITE, HYBRID, etc. |
| `employmentType` | Full-time, Contract, Intern, etc. |
| `salary` | Salary range if found in text |
| `technologies` | Technologies mentioned in the post |
| `emails` | Email addresses found |
| `urls` | Application/company URLs found |
| `isTopLevel` | Whether this is a top-level job post |
| `parentId` | Parent comment ID |
| `replyCount` | Number of replies to this job post |
| `hnUrl` | Direct link to the comment on HN |

## Use cases

- **Job search automation:** Build personalized job alerts from HN hiring threads
- **Tech hiring trends:** Analyze which technologies and companies are hiring most
- **Remote work tracking:** Filter for remote positions across multiple months
- **Recruiting intelligence:** Monitor competitor hiring patterns and tech stack preferences
- **Salary benchmarking:** Extract salary ranges for role and location comparison

## Try the actor on Apify

[Open the HN Who is Hiring Scraper on Apify](https://apify.com/devanshlive/hn-hiring-scraper)

## Related resources

- [Hacker News API documentation](https://github.com/HackerNews/API)
- [Apify Client for JavaScript](https://docs.apify.com/api/client/js/)

## License

MIT
