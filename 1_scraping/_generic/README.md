# Scraping

This folder contains the base scripts for extracting text from web sites, either from the HTML or from downloaded PDFs.

Each folder includes language specific scraping guidelines developed by the each respective language's scraping team.

## Table of Contents:
* [Script Tutorials](#script-tutorials)
  * [HTML](#html) (most common)
  * [PDF](#pdf)
* [Sample Scripts](#sample-scripts)


# Script Tutorials

Offered are two versions of the scraping script. One which scrapes the text off a web page from the HTML (HTML) and one which is designed to extract text from PDFs.

## HTML

This script is available as a Jupyter Notebook script. If you are unfamiliar with Jupyter Notebook, we recommend reviewing the [documentation](https://docs.jupyter.org/en/latest/).

### Configuring the Environment:

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

### Site-Specific Settings:

The first step when scraping a site is to familiarize yourself with its format. What is the site's name? Where are the articles that you want to scrape located? How are the articles organized?

For each site you will need to edit the following variables in the setting cell:

* links_file_path (str): the desired filepath for the links csv to be saved to
* csv_file_path (str): the desired filepath for the text csv to be saved to
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
* type_of_page (str): change the index to match the type of the page that you are trying to scrape (0 for [page](#settings-for-page-sites), 1 for [click_more](#settings-for-show-more-and-scroll-down-sites), and 2 for [scroll_down](#settings-for-show-more-and-scroll-down-sites)). We have organized web sites into 3 main categories:
  * page: when viewing all articles in a section, the articles are organized into separate pages
  * click_more: when viewing all articles in a section, the articles are all on one page, but to view more articles, you must click on a button that says something like "show more" or "show older entries"
  * scroll_down: similar to click_more, but instead of clicking a button, the page adds more articles when you scroll to the bottom of the page

### Settings for Show More and Scroll Down Sites

The following variables must be set if the site is of the type show_more or scroll_down:
* xpath_for_link (str): an XPath that identifies the \<a> tags which contain the links for the articles. This requires inspecting the HTML of the page and identifying the HTML elements which contain the link for each article. In general, this requires specifying the partent element of the a tag.
* xpath_for_link (str): This is only required for show more sites. An XPath that identifies the button that needs to be clicked to show more articles.

### Settings for Page Sites

The following variables must be set if the site is of the type page:
* target_tag (str): the tag for the parent elements of the \<a> tags which contain the links for the articles. This requires inspecting the HTML of the page and identifying the HTML elements which contain the link for each article.
* target_tag_class (str): the class of the parent elements of the \<a> tags which contain the links for the articles.
* page_identifier (str): a string that contains the URL element that identifies a page. For example, the url for page 2 of the International section for the website www.newswebsite.com may look like this: https://www.newswebsite.com/publications/international?page=2. In this case, page_identifier is "?page=" because this is the part of the URL that identifies the page number. Other common page identifers are "/page/" and "?i=".
* pages_each_category (list: int): the number of pages for each section in the urls array. Each element of these array corresponds to its respective element in urls.
* retrieve_amount_of_pages_no_text (bool): Overrides pages_each_category. A boolean setting that can be set to true to automatically identify the number of pages for each section in urls. Used for sites that **do not** show the number of pages in the navigation bar.
* retrieve_amount_of_pages_w_text (bool): Overrides pages_each_category. A boolean setting that can be set to true to automatically identify the number of pages for each section in urls. Used for sites that **do** show the number of pages in the navigation bar.
* xpath_for_amount_of_pages (str): Used if retrieve_amount_of_pages_w_text is true. XPath for the element that contains the number of pages for a section
* index_for_amount_of_pages_location (int): Used if retrieve_amount_of_pages_w_text is true. The starting index for the actual page number in the element that contains the number of pages for a section. Generally 0, but this is needed if there is additional text in the element.
* target_tags (list: str) (optional): a list of tags for the parent elements of the \<a> tags which contain the links for the articles. Used if there are multiple types of tags that contain the article links.
* target_tag_classes (list: str) (optional): a list of classes for the parent elements of the \<a> tags which contain the links for the articles. Used if there are multiple classes for elements that contain the article links.

## PDF

This script is available as a Jupyter Notebook script. If you are unfamiliar with Jupyter Notebook, we recommend reviewing the [documentation](https://docs.jupyter.org/en/latest/).

### Configuring the Environment:

The following installations are required. Please run the following commands for libraries that you do not have:

```
pip install wget
pip install newspaper3k
pip install xmltodict
pip install pandarallel
pip install datefinder
pip install pydrive
pip install selenium
pip install PyPDF2
pip install langdetect
apt-get update # to update ubuntu to correctly run apt install
apt install chromium-chromedriver
cp /usr/lib/chromium-browser/chromedriver /usr/bin
pip3 install pycryptodome
```

Run the import cell to import the required libraries into the script.

### Site-Specific Settings:

The first step when scraping a site is to familiarize yourself with its format. What is the site's name? Where are the articles that you want to scrape located? How are the articles organized?

For each site you will need to edit the following variables in the setting cell:

* links_file_path (str): the desired filepath for the links csv to be saved to
* csv_file_path (str): the desired filepath for the text csv to be saved to
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
* type_of_page (str): change the index to match the type of the page that you are trying to scrape (0 for [page](#settings-for-page-sites), 1 for [click_more](#settings-for-show-more-and-scroll-down-sites), and 2 for [scroll_down](#settings-for-show-more-and-scroll-down-sites)). We have organized web sites into 3 main categories:
  * page: when viewing all articles in a section, the articles are organized into separate pages
  * click_more: when viewing all articles in a section, the articles are all on one page, but to view more articles, you must click on a button that says something like "show more" or "show older entries"
  * scroll_down: similar to click_more, but instead of clicking a button, the page adds more articles when you scroll to the bottom of the page
* pdf_url_endpoint (str): base url for PDF host endpoint. Usually this is the same as host, but in some cases the website that the PDFs are hosted on is different from the host URL
* intermediate_site (bool): If true, there is an intermediate site between the page where links were extracted and the actual PDF download. Essentially, true unless the link on the page with the list of PDFs links directly to a PDF download
* title_tag (str): the tag for the element which contain the title of the article on an intermediate page
* title_class (str): the class for the element which contain the title of the article on an intermediate page
* pdf_id_tag (str): the tag for the parent element of the \<a> element which contains the PDF download link
* pdf_id_class (str): the tag for the parent element of the \<a> element which contains the PDF download link

### Settings for Show More and Scroll Down Sites

The following variables must be set if the site is of the type show_more or scroll_down:
* xpath_for_link (str): an XPath that identifies the \<a> tags which contain the links for the articles. This requires inspecting the HTML of the page and identifying the HTML elements which contain the link for each article. In general, this requires specifying the partent element of the a tag.
* xpath_for_link (str): This is only required for show more sites. An XPath that identifies the button that needs to be clicked to show more articles.

### Settings for Page Sites

The following variables must be set if the site is of the type page:
* target_tag (str): the tag for the parent elements of the \<a> tags which contain the links for the articles. This requires inspecting the HTML of the page and identifying the HTML elements which contain the link for each article.
* target_tag_class (str): the class of the parent elements of the \<a> tags which contain the links for the articles.
* page_identifier (str): a string that contains the URL element that identifies a page. For example, the url for page 2 of the International section for the website www.newswebsite.com may look like this: https://www.newswebsite.com/publications/international?page=2. In this case, page_identifier is "?page=" because this is the part of the URL that identifies the page number. Other common page identifers are "/page/" and "?i=".
* pages_each_category (list: int): the number of pages for each section in the urls array. Each element of these array corresponds to its respective element in urls.
* retrieve_amount_of_pages_no_text (bool): Overrides pages_each_category. A boolean setting that can be set to true to automatically identify the number of pages for each section in urls. Used for sites that **do not** show the number of pages in the navigation bar.
* retrieve_amount_of_pages_w_text (bool): Overrides pages_each_category. A boolean setting that can be set to true to automatically identify the number of pages for each section in urls. Used for sites that **do** show the number of pages in the navigation bar.
* xpath_for_amount_of_pages (str): Used if retrieve_amount_of_pages_w_text is true. XPath for the element that contains the number of pages for a section
* index_for_amount_of_pages_location (int): Used if retrieve_amount_of_pages_w_text is true. The starting index for the actual page number in the element that contains the number of pages for a section. Generally 0, but this is needed if there is additional text in the element.
* target_tags (list: str) (optional): a list of tags for the parent elements of the \<a> tags which contain the links for the articles. Used if there are multiple types of tags that contain the article links.
* target_tag_classes (list: str) (optional): a list of classes for the parent elements of the \<a> tags which contain the links for the articles. Used if there are multiple classes for elements that contain the article links.

# Sample Scripts

Stub