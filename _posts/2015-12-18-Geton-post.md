---
layout: post
title: How I developed GetOn?
tags: [geton, silverFoxA, Android, Google Play Store]
subtitle: GetOn is one of my experimental app to explore new areas.
---
<font size="4" style="font-weight:600;">Date - 19/12/2015</font>
<font size="4">Making an application seems interesting but it’s a storm for the brain where you are not allowed to think about one particular class but you have entire pacific ocean to explore.This particular experimental app is meant for polishing my web scrapping skill as well as to write some efficient code. When you are talking about scrapping, it always mean re-organizing other’s mess for betterment. The same stands for data scrapping or web scrapping as well.<br/>For this particular app I will be scrapping<br/> <ul><li><font size="4"><a href="http://www.google.com">Google</a></font></li><li><font size="4"><a href="http://www.twitter.com">Twitter</a></font></li><li><font size="4"><a href="http://www.instagram.com">Instagram</a></font></li></ul><br/>These three are the source of information for all sort of data. From these three site one can retrieve almost all possible data that one can ever need if it's presented in proper manner.</font><p><font size="4">Just like any other software development, this particular app will follow the same life cycle and I will be blogging about the entire life cycle as I made progress on this application</font></p>

<div>
<span class="quote">"I believe We have Rights to information"</span></div><br/> <font size="4">The application stands for the same as well.<br/>Scrapping Google isn't an easy task when all the items that they have are completely dynamic and you barely get any left over. Moreover <strong>if Google detects you scrapping their data they will first warn you and later they will block you temporarily. <a href="http://google-scraper.squabbel.com/">source</a></strong>. Today I tried scrapping the web but unfortunately couldn't retrieve any data whatsoever I needed for the web.</font>
<br/><font size="4">For this particular experiment I have used <a href="http://www.jsoup.org">Jsoup</a></font><br/>
<pre>
package JsoupPrac;

import java.io.IOException;

import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.select.Elements;

public class GoogleScrapper {
	public static void main(String[] args) throws IOException{
		  String url = "https://www.google.co.in/#q=query";
	        Document document = Jsoup.connect(url) .userAgent("Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/33.0.1750.152 Safari/537.36")
	        	     .get();
	        //System.out.println(document+"");
	        Elements answerers = document.select("div ._mr .kno-fb-ctx");
	        System.out.println(answerers+"");
	        for (Element answerer : answerers) {
	            System.out.println("Answerer: " + answerer.text());
	        }
	        //Unfortunately it provides no result
	}
}

</pre>

<font size="4">I got few alternative though, which is yet to be explored. </font>
<ul>
<li><font size="4">I came accross this website where this person, Amit Agarwal tries to explain how to scrap using excel/ google sheets  <a href="http://www.labnol.org/internet/google-web-scraping/28450/">source</a></font></li>
<li><font size="4">Another way that came to my mind is to download the entire source of the file that we get when inspecting. So for each search basically I will be saving the entire page and Google need not know that I'm scrapping as it will be one search. [just a thought]</font></li>
</ul>

<font size="4">As per Amit Agarwal
<pre>
Google Search Scraper using Google Spreadsheets

If you ever need to extract results data from Google search, there’s a free tool from Google itself that is perfect for the job. It’s called Google Docs and since it will be fetching Google search pages from within Google’s own network, the scraping requests are less likely to get blocked.
</pre>
</font>

<font size="4">Okay so my idea doesn't seem to be working at this moment, as Google has it's own way of pushing their code for some obvious reason though <a href="http://stackoverflow.com/questions/16078926/how-does-google-hide-html-source-of-search-results">source</a></font>
<pre>
Google builds the DOM with the javascript you noted. It does this for a number of reasons:

Decrease the load on the server to generate each dynamic result set with HTML markup.
Google returns the results in a JSON feed (example) - pastebin. Less processing power is required to produce the JSON response than a full HTML snippet or completely new page
Speed. Assuming that the user has a decent internet connection, the speed of the pages rendering on the client side compared to the server side is negligible.
</pre>

<pre>Google does that by generating the page using a pile of client side JavaScript. It's almost certainly a side effect, not a design goal.</pre>

<font size="4">There's also a project in PHP available <a href="http://scraping.compunect.com/">source</a> but I won't be looking into it for two reason 
<ul><li><font size="4">It's my experiment so I want to develope something of my own.</font></li><li><font size="4">Integrating Java and Php can be trickier.</font></li></ul></font>

<font size="4">So finally got something but it's not what I need for my app, hence we don't have the option to extract whatever data we want so far</font>
<pre>
package JsoupPrac;

import java.io.IOException;

import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.select.Elements;

public class GoogleScrapper {
	public static void main(String[] args) throws IOException{
		Document doc;
	    try{
	        doc = Jsoup.connect("https://www.google.co.in/search?as_q=&as_oq=query&as_eq=&as_nlo=&as_nhi=&lr=lang_en&cr=countryCA&as_qdr=all&as_sitesearch=&as_occt=any&safe=images&tbs=&as_filetype=&as_rights=&gws_rd=cr&ei=4Id1Vs7pC8rQjwOEkbP4CA#lr=lang_en&cr=countryCA&as_qdr=all&tbs=lr:lang_1en%2Cctr:countryCA&q=query").userAgent("Mozilla").ignoreHttpErrors(true).timeout(0).get();
	        Elements links = doc.select("ol[class=g]");
	        for (Element link : links) {
	            Elements titles = link.select("h3[class=r]");
	            String title = titles.text();

	            Elements bodies = link.select("span[class=st]");
	            String body = bodies.text();

	            System.out.println("Title: "+title);
	            System.out.println("Body: "+body+"\n");
	        }
	    }
	    catch (IOException e) {
	        e.printStackTrace();
	    }
	}
}

</pre>

<font size="4">Okay don't ask me how but this seems to be working now</font>
<pre>
public static void main(String[] args) throws IOException{
		Document doc;
	    try{
	        doc = Jsoup.connect("https://www.google.co.in/search?as_q=&as_oq=query&as_eq=&as_nlo=&as_nhi=&lr=lang_en&cr=countryCA&as_qdr=all&as_sitesearch=&as_occt=any&safe=images&tbs=&as_filetype=&as_rights=&gws_rd=cr&ei=4Id1Vs7pC8rQjwOEkbP4CA#lr=lang_en&cr=countryCA&as_qdr=all&tbs=lr:lang_1en%2Cctr:countryCA&q=query").userAgent("Mozilla").ignoreHttpErrors(true).timeout(0).get();
	        Elements links = doc.select("#rhs_block");
	        for (Element link : links) {
	        	Elements again = link.select("._B5d");
	        		System.out.println(again.text().toString());
	        }
	    }
	    catch (IOException e) {
	        e.printStackTrace();
	    }
	}
</pre>

<font size="4" style="font-weight:600;">Date - 22/12/2015</font>
<font size="4">Okay I was bit busy lately with the club we going to organise in our college hence didn't get time to work on this project. Fortunately I have good news. Okay so I asked a question on stackoverflow <a href="http://stackoverflow.com/questions/34389558/how-can-i-retrieve-the-inspected-source-code-google-chrome-in-java/34389583#34389583">link</a>, the answer here is exactly what we are looking for but for the time being we are not yet sure how we shall implement the same in a smartphone. For this we will ne needing <a href="http://www.seleniumhq.org/download/">Selenium</a> make sure to download the Selenium Standalone Server and Selenium Client & WebDriver Language Bindings also we will be needing a web driver for the browser you would like to work on.</font>

<pre>
WebDriver driver;
		System.setProperty("webdriver.chrome.driver","/Users/Avi/Downloads/chromedriver");
        driver= new ChromeDriver();   
		driver.get(url);
		Document doc = Jsoup.parse(driver.getPageSource());
		System.out.println(doc);
	    Elements links = doc.select("a.fl");
	        for (Element link : links) {
	        		System.out.println(link.attr("abs:href").toString());
	        }
</pre>
<font size="4">So now basically we have all the source code in HTML which we can parse using Jsoup.</font>

<font size="4" style="font-weight:600;">Date - 23/12/2015</font>
<font size="4">As per the comment on my stackoverflow post from alecxe, the same can be implemented with mobile browser as well.</font>
<pre>
You can actually automate mobile browsers via selenium and appium
</pre>

<font size="4">Now we need to look into the following <ul><li>Selenium</li><li>appium</li></ul></font>

<font size="4">[To be Continued...]</font>
<br/>
