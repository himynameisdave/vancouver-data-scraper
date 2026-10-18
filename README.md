# vancouver-data-scraper

Very simple scrapers for various public Vancouver datasets. These make use of GitHub actions which are set to run on a cron schedule, then simply commit the changes to the dataset to the repository (if there are any). Inspired by [this post](https://simonwillison.net/2020/Oct/9/git-scraping/).

Plans to eventually add something to tweet or post these changes out to the world is something I'd like to eventually add here.

### Building permits

The City's `issued-building-permits` export is nondeterministic: on each refresh a few hundred older permits (mostly 2016 to 2018) swap between alternate `issuedate` values, or drop out of the file and come back. Because of this, the diff of `data/building-permits.json` is noisy and mostly isn't new permits.

`data/building-permits-recent.json` holds only permits whose `permitnumbercreateddate` is on or after the cutoff in [the workflow](.github/workflows/building-permits.yml). Records only enter this file when a permit is newly issued, so any added record is a new permit. Existing records can still have their `issuedate` change in place. Use this file to drive any "what's new" automation, and keep `data/building-permits.json` as the full archive.

Makes use of Git LFS for larger files. Read more about that [here](https://docs.github.com/en/repositories/working-with-files/managing-large-files/installing-git-large-file-storage).

This data is licensed under the [Open Government License - Vancouver](https://opendata.vancouver.ca/pages/licence/).
