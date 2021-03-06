# AO3Scraper

In collaboration with [@ssterman](https://github.com/ssterman). A simple Python [Archive of Our Own](https://archiveofourown.org/) scraper.

Features:
- Given a fandom URL and amount of fic you want, returns a list of the fic IDs. (ao3_work_ids.py)
- Given a (list of) fic ID(s), saves a CSV of all the fic metadata and content. (ao3_get_fanfics.py)
- Given the CSV of fic metadata and content, saves a new CSV of only the metadata. (extract_metadata.py)
- Given the CSV created by ao3_get_fanfics.py, creates a folder of individual text files containing the body of each fic (csv_to_txts.py)

## Dependencies
- pip install bs4
- pip install requests
- pip install unidecode

## Example Usage

Let's say you wanted to collect data from the first 100 English completed fics, ordered by kudos, in the Sherlock (TV) fandom. The first thing to do is use AO3's nice search feature on their website.

We get this URL as a result: http://archiveofourown.org/works?utf8=%E2%9C%93&work_search%5Bsort_column%5D=kudos_count&work_search%5Bother_tag_names%5D=&work_search%5Bquery%5D=&work_search%5Blanguage_id%5D=1&work_search%5Bcomplete%5D=0&work_search%5Bcomplete%5D=1&commit=Sort+and+Filter&tag_id=Sherlock+%28TV%29 

Run `python ao3_work_ids.py`. The command line interface will ask you for this URL, how many fics IDs you want (100), and a .csv output file (let's call it sherlock.csv).

Now, to actually get the fics, run `python ao3_get_fanfics.py sherlock.csv`. You can optionally add some flags: 
- `--csv output.csv` (the name of the output csv file, default fanfic.csv)
- `--header 'Chrome/52 (Macintosh; Intel Mac OS X 10_10_5); Jingyi Li/UC Berkeley/email@address.com'` (an optional http header for ethical scraping)

If you don't want to give it a .csv file name, you can also query a single fic id, `python ao3_get_fanfics.py 5937274`, or enter an arbitrarily sized list of them, `python ao3_get_fanfics.py 5937274 7170752`.

If you stop a scrape from a csv partway through (or it crashes), you can restart from the last uncollected work_id using the flag `--restart 012345` (the work_id).  The scraper will skip all ids up to that point in the csv, then begin again from the given id. 

By default, we save all chapters of multi-chapter fics. We cannot scrape fics that are locked (for registered users only), but submit a pull request if you want to do authentication! 

Note that the 5 second delays before requesting from AO3's server are in compliance with the AO3 terms of service.  Please do not remove these delays.  

Happy scraping! 

##Improvements

We love pull requests. Some known issues, all around data clean-up / CSV saving:
- All fic content is converted to ASCII, which strips quotations if they're curly (’ instead of '). CSV writer should be able to support unicode. 
- Related: without unicode, it cannot scrape non-English characters, e.g. fics in Chinese.  These are currently skipped and the ids saved to a separate csv
- Viewing the csv in Excel causes some weird line break issues in fic content. (Viewing it in the Mac preview viewer is fine).

##FF.net

Want to scrape fanfiction.net? Check out my friend [@smilli](https://github.com/smilli/)'s [ff.net scraper](https://github.com/smilli/fanfiction)! 
