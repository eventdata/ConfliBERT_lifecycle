# Scraping

This folder contains the base scripts for extracting text from web sites, either from the HTML or from downloaded PDFs.

Each folder includes language specific scraping guidelines developed by the each respective language's scraping team.

## Table of Contents:
* [Script Tutorials](#script-tutorials)
  * [HTML](#html) (most common)
  * [PDF](#pdf)
* [Sample Scripts](#sample-scripts)


# Script Tutorials

Offered are two versions of the scraping script. One which scrapes the text directly from a website's [HTML](#html) and one which is designed to extract text from [PDFs](#pdf).

# Quick Aside if you are not familiar with HTML files or XPaths:

This script relies on identifying elements in an HTML file using an XPath or by identifying the element's tag and class. If you are not familiar with how an HTML file works, check out [this website](https://blog.hubspot.com/website/html) for an introduction.

You will need to identify either the element or the parent element that contains the target text or link for a step in scraping. For example, you may need to identify a series of links on a page that lead to an article or PDF. When you see an example of the desired object (such as a link) on the page, you can right click on it and click "Inspect Element" to open that part of the page in HTML. In HTML, this will look something like this:

```
...
<div class="article-preview">
  <h3>A really awesome article!!!</h3>
  <img src="awesome_image.png"/>
  <p>Short decription of this awesome article</p>
  <a href="https://newswebsite.com/politics/2023/12342.html">Click here to read more</a>
</div>
...
```

In this example, we want to identify the link to the article, "https://newswebsite.com/politics/2023/12342.html". In order to do this we need to identify the parent element of the link, which is div. This div has a class: "article-preview" which we can use to further specify it.

Therefore, in the script we will either identify the element as something like:

```
example_tag = "div"
example_class = "article-preview"
```
or:
```
example_XPath = "//div[@class='article-title']/a"
```

If you are not familiar with XPaths, check out [this website](https://www.w3schools.com/xml/xpath_intro.asp) for an introduction. The above XPath is the most common type you will use.

A quick explanation of the above XPath:
* // means look through the entire file, regardless of its relationship to the root
* div means look for an element with the tag div
* [@class='article-title'] meeans "with the class 'article-title'"
* /a means "look for the element a with the previous parent"

All together this means "look through the entire HTML file for an element a with the parent div with the class 'article-title'".

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

### Steps:

1. The first step is to complete all required settings in the configuration cell. All settings variables are detailed in the next section. The workflow of the script will be affected by 2 main distinctions:
  
    * How are the PDFs organized on the website? There are 3 types of organization:
          
      * Page: The PDFs are organized in pages with page numbers somewhere on the page. This is the most common type of website.
      * Show More: All of the PDFs are organized on one page, but you must click a button to show more PDFs.
      * Scroll Down: Similar to Show More, but instead of a button to show more PDFs, more PDFs are automatically loaded when you scroll to the bottom of the page.
    * How many steps are required to access the PDF file? In order to do the actual scraping of the PDF, the script will need to get a link to a PDF download. In the simplest case, there is a link directly to download the PDF on the section of the site where PDFs are listed. In this case, you will be able to skip some steps and you can set intermediate_site to False. However, in many cases, the link for each PDF will be a link to an info page which will then contain a link to download the PDF. In this case, you will need to set intermediate_site to True and you will need to do an additional pre-scraping step.
2. Run all of the cells up to the cell that begins with "Pages = []", most of these cells will define the required functions for scraping. However, if you elected to automatically detect the number of pages for each section, this is where that process will happen. If there are any issues with automatic detection of the number of pages, those will appear here.
3. Run the cell that begins with "Pages = []", this cell will either populate a dataframe with a list of pages to scrape links from, or in the case of the types Show More and Scroll Down, this is where the links will be scraped.
4. Run all of the cells up to the cell that begins with "if type_of_page == 'page':". These cells also just define some required functions for scraping. If you forget to run these, you will get an undefined function error.
5. Run the cell that begins with "if type_of_page == 'page':". If the type is Page, this cell will scrape all of the pages for links to PDFs. If the type is Show More or Scroll Down, this cell will just reformat the dataframe that already contains the links.
6. Run all of the cells up to the cell that begins with "SKIP IF THE PDFS...". These cells will reformat the links dataframe and save your scraped links to a csv file in case you need to restart this process from here. If there are any issues getting the links, those problems will be apparent here. Check that the dataframe does contain links. Try accessing a few of these links in your browser to check if they are working links and that they link to what you expect them to. If your dataframe seems correct, continue to the next step.
7. If the links that are currently in your dataframe link to an intermediate site, you will need to run the next cell. Otherwise, you can skip this cell. Ensure that intermediate_site is set to True. This cell will go to each scraped link and record the title of the PDF and find the download link for the PDF. The download link will be in the field "pdf_id" in the dataframe.
8. The next cell will remove all links that did not have a download link and filter the PDFs based on the title. Many PDFs are in English, even on Spanish sites, so this step is useful to filter out any English language PDFs. Do not run this if there is not an intermediate site. If this step removes an unexpected number of PDFs, double check that your XPaths are correct and that the PDF IDs are what you expect them to be.
9. The next two cells will create a checkpoint of the dataframe with the PDF IDs. This is useful as the previous two steps may take a significant amount of time. This checkpoint will allow you to restart from here rather than completely restart. There is no reason to do this step if you do not have an intermediate site.
10. Run the cell that begins with "Run parallel text...". This is where the actual PDF text scraping happens. This step should take the longest out of any step as PDF files can be quite large, so this step is bottlenecked by internet speed and the PDF Parser. If there are any issues here, there are likely issues with your scraped links or PDF IDs. Ensure that the links/PDF IDs do indeed link to a PDF. Some errors are fine here. For example, some PDFs may have an incorrect format, so Unexpected EOF errors are expected. This error will also happen if the downloaded file is not a PDF, such as an HTML page, JPEG, or PNG. If the link is to an HTML page, you may want to use the HTML scraping script. If the downloaded file is a JPEG or PNG, it is not possible to scrape those files with this script. If most PDFs are not giving errors, this step is likely working correctly.
11. Run the next cell to print out the dataframe with the scraped text. Check that there is actually text in the text field and that it is not nonsensical. If the text field looks good, you can run the rest of the cells to clean up the dataframe and save it. You have now scraped a website for PDFs!

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