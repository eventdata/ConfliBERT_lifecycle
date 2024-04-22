# Scraping

This folder contains the base script for extracting text from web sites, either from the HTML or from downloaded PDFs. Two sample scripts are included as well as instructions for running the script on a website.

Each folder includes language specific scraping guidelines developed by the each respective language's scraping team.

## Table of Contents:
* [Script Tutorials]()
  * [HTML](html) (most common)
  * PDFs


## Script Tutorials

Offered are two versions of the scraping script. One which scrapes the text off a web page from the HTML (HTML) and one which is designed to extract text from PDFs.

### HTML

This script is available as a Jupyter Notebook script. If you are unfamiliar with Jupyter Notebook, we recommend reviewing the [documentation](https://docs.jupyter.org/en/latest/).

#### Configuring the Environment:

The following installations are required. Please run the following commands for libraries that you do not have:

```
pip install wget
pip install newspaper3k
pip install xmltodict
pip install pandarallel
pip install datefinder
pip install pydrive
pip install selenium
pip3 install "urllib3 <=1.26.15"
apt-get update # to update ubuntu to correctly run apt install
apt install chromium-chromedriver
cp /usr/lib/chromium-browser/chromedriver /usr/bin
```

Run the import cell to import the required libraries into the script.

#### Site-Specific Settings:

The first step when scraping a site is to familiarize yourself with its format. What is the site's name? Where are the articles that you want to scrape located? How are the articles organized?

For each site you will need to edit the following variables in the setting cell:

* news_outlet (str): The name of the site.
* country (str): The country of origin for the site.
* max_click_SHOW_MORE (int): The maximum amount of times that the script will click a button to show more articles.
* host (str): URL for the site. Generally should not include any sections (e.g. "/publications").
* urls (list: str): A list of every section on the site that needs to be scraped
  * For example, for a site with the host https://www.newswebsite.com, you may want to scrape the International and Politics section of the website.
    * Click on those sections on the website and look for the additional sections in the url. For example, the url for the International sections could be https://www.newswebsite.com/publications/international and the url for the Politics sections could be https://www.newswebsite.com/politics.
    * The additional part of the URL after the slash is the section that you want for the urls list. For the International this would be "/publications/international" and for the Politics sections this would be "/politics".
    * To scrape these two hypothetical sections, urls should be:
      * ["/publications/international", "/politics"]
* type_of_page (str): change the index to match the type of the page that you are trying to scrape (0 for page, 1 for click_more, and 2 for scroll_down). We have organized web sites into 3 main categories:
  * page: when viewing all articles in a section, the articles are organized into separate pages
  * click_more: when viewing all articles in a section, the articles are all on one page, but to view more articles, you must click on a button that says something like "show more" or "show older entries"
  * scroll_down: similar to click_more, but instead of clicking a button, the page adds more articles when you scroll to the bottom of the page
* 



### PDFs

This script is available as a Jupyter Notebook script. If you are unfamiliar with Jupyter Notebook, we recommend reviewing the [documentation](https://docs.jupyter.org/en/latest/).

Stub