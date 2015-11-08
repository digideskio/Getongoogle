# ongoogle
seach feedback
#labels Featured,Phase-Deploy
= Overview =
The new Search Quality Feedback Toolkit provides your users with a quick and easy way to provide feedback on their search experience.  The design is light-weight and the feedback boxes automatically disappear and show a confirmation (Thank You) message once the email was sent successfully.

= System Requirements =
Since the bulk of the logic is all encompassed in the provided javascript file (`feedabck.js`), this project could easily be adapted to any web serving stack you already have in place.  However to make it easier for you to visualize and implement a fully integrated solution, we've described the full setup including how to add the packaged XAMPP server side components if you don't have them.

The following are required to get the Search Quality Feedback Toolkit installed and running against the provided sample data:

    * Apache HTTP Server 2.2.4 or later
    * PHP 5.2.3 or later

We've referenced an easy packaged installation process for these below.  You can skip that section if you already have these available.

= Contents =
  # readme.txt (this file)
  # feedback.js (the main javascript file with the Search Quality Feedback Toolkit logic)
  # feedback.css (the stylesheet that formats the Quick Feedback box, for example)
  # send-feedback.php (server component that connects to your email server and sends the emails)
  # sample.html (sample page that lets you test the various Feedback Toolkit apps)


= Getting Started =
To get started with the Search Quality Feedback Toolkit just follow these simple steps:

   # If you don't already have the prerequisite Apache HTTP Server and PHP installed and available, see the section below to conveniently install them auto-configured to work together.
   # Download and extract the distribution of this project from http://code.google.com/p/search-feedback/downloads/list into a folder under <xampp-install-dir>/xampplite/htdocs/
   # Edit line 32 of `feedback.js` to provide the absolute location of your `send-feedback.php` file (e.g. `http://YOUR_HOST/<feedback directory>/send-feedback.php`).
   # Change the email addresses in `feedback.js` (should be in 2 locations (lines 202 & 203)) to display to your users where the Quick Feedback emails will be going (we recommend creating a mailing list specifically for this purpose (e.g. quick-search-feedback@mycompany.com) so you can easily set up mail filter rules on these emails).  You don't have to expose this email address if you don't want.  You can just remove this portion of the HTML code.
   # Change the email addresses in `send-feedback.php` (should be in 2 locations (lines 16 & 17)) to higlight where you would like the Quick Feedback emails to go and where you would like the `Did you find these results useful?` emails to go.  (we recommend creating two separate mailing list specifically for this purpose (e.g. quick-search-feedback@mycompany.com & search-quality-feedback@mycompany.com) so you can easily set up mail filter rules on these emails).
   # In line 35 of `send-feedback.php`, we hard-code the 'From' email address to be a fake anonymous one.  However, if your browser session has login information or username information or security information that would allow you to get the user's information, we recommend that you incorproate that logic here.  The benefit of that is that when you get feedback, you will know who sent it, you'll be able to easily reply to the feedback email, etc.  In addition, it will save the user from having to enter their email address in the search box _and_ it will save you some clutter so you won't have to remind users to enter their email address in the feedback box.  All features will work fine if you don't do this.  We are just showing you where you would do it if you could.
   # To test that the Quick Feedback functionality is working, launch your favorite browser and run http://localhost/<feedback directory>/sample.html?q=test%20query (we are adding the q= parameter to simulate this working in a search environment)
   # If everything is working correctly, you should see a Quick Feedback box, properly formatted.  Try typing in some feedback and pressing send.  If everything is working correctly, the feedback box will disappear and give you a confirmation that the feedback was sent properly.
   # Now check your email.  You should have received an automated email with the feedback embedded.
   # Now edit your `sample.html` file and change {{{<body onload="feedback.quickFeedbackShow()">}}} to {{{<body onload="feedback.usefulResultsShow()">}}}.
   # To test that the search quality feedback is working, launch your favorite browser and run http://localhost/<feedback directory>/sample.html?q=test%20query (we are adding the q= parameter to simulate this working in a search environment)
   # If everything is working correctly, you should see a line asking if you found the results useful.  Try clicking yes.  If everything is working correctly, the line will disappear and give you a confirmation that the feedback was sent properly.
   # Check your email again.  You should have received an automated email with the feedback embedded.
   # Now refresh your browser.  You should see the same line asking if you found the results useful.  This time, try clicking no.  If everything is working correctly, the line will disappear and ask you to elaborate.  Add some more information about why the results weren't useful and press enter.  You should receive a confirmation that the feedback was sent properly.
   # Check your email again.  You should have received TWO more automated emails with the feedback embedded.  The first one shows the 'no' and the second shows the elaboration.  We designed it this way so users would have the flexibility of just entering 'no' and not elaborating (similar to a simple voting mechanism).  Of course, this can be re-designed using the javascript and PHP code.


= Installation of Apache HTTP Server & PHP =
If you don't already have Apache and PHP installed, then the XAMPP project makes it easy to get them installed and configured to run together.

Here are the steps for Windows environments:

   # Download the XAMPP Lite zip file (1.6.3a or later) from http://www.apachefriends.org/en/xampp-windows.html#646
   # Extract the zip file into a local directory, we'll call this directory <xampp-install-dir>
   # Run <xampp-install-dir>/xampplite/setup_xampp.bat
   # Run <xampp-install-dir>/xampplite/apache_start.bat to start the web server (If you get an error message it may be that you have another web server running that you'll need to shut down.  Check this FAQ article to help troubleshoot and remember to re-run the apache_start script above once the issue has been resolved.)
   # Verify that the Apache HTTP server is running correctly by loading http://localhost/ into your web browser and look for the XAMPP logo

This setup is for development purposes only.  You should take the necessary steps to security harden your installation of these components before pushing this to a production environment.  More info about doing this is at http://www.apachefriends.org/en/xampp-windows.html#1221

The analogous steps for Linux environments are similar and are described in detail at http://www.apachefriends.org/en/xampp-linux.html#374.  And the security hardening steps are described here: http://www.apachefriends.org/en/xampp-linux.html#381.


= Adding both methods of gathering feedback to your Google Mini or Google Search Appliance Front End =

_This is the recommended option._

First, create a new test front end. In this new test front end, search for the section titled: `Output all results` and replace all the text in that section (i.e. up to the section titled `Stopwords suggestions`) with the following:
{{{
<xsl:template name="results">
  <xsl:param name="query"/>
  <xsl:param name="time"/>

  <!-- *** Add top navigation/sort-by bar *** -->
  <xsl:if test="($show_top_navigation != '0') or ($show_sort_by != '0')">
    <xsl:if test="RES"> <!-- there might be onebox results but no RES  -->
      <table width="100%">
      <tr>
      <xsl:if test="$show_top_navigation != '0'">
      <td align="left">
        <xsl:call-template name="google_navigation">
        <xsl:with-param name="prev" select="RES/NB/PU"/>
        <xsl:with-param name="next" select="RES/NB/NU"/>
        <xsl:with-param name="view_begin" select="RES/@SN"/>
        <xsl:with-param name="view_end" select="RES/@EN"/>
        <xsl:with-param name="guess" select="RES/M"/>
        <xsl:with-param name="navigation_style" select="'top'"/>
        </xsl:call-template>
      </td>
      </xsl:if>
      <xsl:if test="$show_sort_by != '0'">
      <td align="right">
      <xsl:call-template name="sort_by"/>
      </td>
      </xsl:if>
      </tr>
    </table>
    </xsl:if>
  </xsl:if>

  <!-- *** Handle OneBox results, if any ***-->
  <xsl:if test="$show_onebox != '0'">
    <xsl:if test="/GSP/ENTOBRESULTS">
      <xsl:call-template name="onebox"/>
    </xsl:if>
  </xsl:if>

  <!-- *** Handle spelling suggestions, if any *** -->
    <xsl:if test="$show_spelling != '0'">
      <xsl:call-template name="spelling"/>
    </xsl:if>

  <!-- *** Handle synonyms, if any *** -->
    <xsl:if test="$show_synonyms != '0'">
      <xsl:call-template name="synonyms"/>
    </xsl:if>

  <!-- *** Output Google Desktop results (if enabled and any available) *** -->
        <xsl:if test="$egds_show_desktop_results != '0'">
            <xsl:call-template name="desktop_results"/>
        </xsl:if>

  <!-- *** Output results details *** -->
    <div>
    <!-- for keymatch results -->
    <xsl:if test="$show_keymatch != '0'">
      <xsl:apply-templates select="/GSP/GM"/>
    </xsl:if>



<table width="100%"><tr><td>

    <!-- for real results -->
    <xsl:apply-templates select="RES/R">
      <xsl:with-param name="query" select="$query"/>
    </xsl:apply-templates>

 </td><td valign='top' align='right'>
	
		<style type="text/css">
		table.quickFeedbackBorderStyle{
			border-width: 1px 1px 1px 1px;
			border-spacing: 2px;
			border-style: outset outset outset outset;
			border-color: gray gray gray gray;
			border-collapse: separate;
			background-color: white;
		}
		</style>
		
		<link rel="stylesheet" href="http://{FULL_PATH}/feedback.css" type="text/css" media="screen"/>
		
		<script type="text/javascript" src="http://{FULL_PATH}/feedback.js"/>
		
		<div id="quickFeedbackBox">
		<table class='quickFeedbackBorderStyle' align='RIGHT'><tr><td>
			  <span id='quickFeedback'>Please provide feedback on your search experience.
			  <br></br>Please include your email address so we can contact you if necessary.<p></p></span>
			  <textarea tabindex='1' id='quickFeedbackInput'></textarea>
			    <div style='position: relative;'>
			    <input class='send' tabindex='3' onclick='return feedback.quickFeedbackSend()' 
			    type='submit' value='Send' /> <span id='quickFeedback'>
			    This will be sent to <a tabindex='-1' href='mailto:feedback@mycompany.com'>
			    feedback@mycompany.com</a>.</span>
			    </div>
		</td></tr></table>
		</div>

</td></tr></table>


  <!-- *** Filter note (if needed) *** -->
    <xsl:if test="(RES/FI) and (not(RES/NB/NU))">
      <p>
        <i>
        In order to show you the most relevant results, we have omitted some entries very similar to the <xsl:value-of select="RES/@EN"/> already displayed.<br/>If you like, you can <a href="{$filter_url}0"> repeat the search with the omitted results included</a>.
        </i>
      </p>
    </xsl:if>
    </div>

  <!-- *** Add bottom navigation *** -->
    <xsl:variable name="nav_style">
      <xsl:choose>
        <xsl:when test="($access='s') or ($access='a')">simple</xsl:when>
        <xsl:otherwise>
          <xsl:value-of select="$choose_bottom_navigation"/>
        </xsl:otherwise>
      </xsl:choose>
    </xsl:variable>

    <xsl:call-template name="google_navigation">
      <xsl:with-param name="prev" select="RES/NB/PU"/>
      <xsl:with-param name="next" select="RES/NB/NU"/>
      <xsl:with-param name="view_begin" select="RES/@SN"/>
      <xsl:with-param name="view_end" select="RES/@EN"/>
      <xsl:with-param name="guess" select="RES/M"/>
      <xsl:with-param name="navigation_style" select="$nav_style"/>
    </xsl:call-template>


    <p id='usefulResults'>
    <span id='usefulResultsContent'>
    Did you find these results useful?<br></br>
    <a id='usefulResultsYes'
    onclick='return feedback.usefulResultsYes()'
    href=''>Yes</a> <span class='desc'> | </span>  <a id='usefulResultsNo'
    onclick='return feedback.usefulResultsNo()'
    href=''>No</a>
    </span>
    <br></br></p>


  <!-- *** Bottom search box *** -->
    <xsl:if test="$show_bottom_search_box != '0'">
      <xsl:call-template name="bottom_search_box"/>
    </xsl:if>

</xsl:template>

}}}

Make sure to change the 2 {FULL_PATH} placeholders to be the full path of where your css & javascript files reside.

Make sure to change the 'feedback@mycompany.com' email addresses to be the email address where you would like to send your Quick Feedback.

If you run a diff on this XSLT code and the default XSLT code for this section, you will see that it simply adds a table around the search results, puts the Quick Feedback box in the right column of that table aligned to the top, and adds a feedback line to the bottom of the search results.

Now, once your test front end works, you can perform the same search and replace in your current front end stylesheet or custom front end stylesheet. Enjoy!



= Adding only the Quick Feedback box to your Google Mini or Google Search Appliance Front End =
First, create a new test front end. In this new test front end, search for the section titled: `Output all results` and replace all the text in that section (i.e. up to the section titled `Stopwords suggestions`) with the following:
{{{
<xsl:template name="results">
  <xsl:param name="query"/>
  <xsl:param name="time"/>

  <!-- *** Add top navigation/sort-by bar *** -->
  <xsl:if test="($show_top_navigation != '0') or ($show_sort_by != '0')">
    <xsl:if test="RES"> <!-- there might be onebox results but no RES  -->
      <table width="100%">
      <tr>
      <xsl:if test="$show_top_navigation != '0'">
      <td align="left">
        <xsl:call-template name="google_navigation">
        <xsl:with-param name="prev" select="RES/NB/PU"/>
        <xsl:with-param name="next" select="RES/NB/NU"/>
        <xsl:with-param name="view_begin" select="RES/@SN"/>
        <xsl:with-param name="view_end" select="RES/@EN"/>
        <xsl:with-param name="guess" select="RES/M"/>
        <xsl:with-param name="navigation_style" select="'top'"/>
        </xsl:call-template>
      </td>
      </xsl:if>
      <xsl:if test="$show_sort_by != '0'">
      <td align="right">
      <xsl:call-template name="sort_by"/>
      </td>
      </xsl:if>
      </tr>
    </table>
    </xsl:if>
  </xsl:if>

  <!-- *** Handle OneBox results, if any ***-->
  <xsl:if test="$show_onebox != '0'">
    <xsl:if test="/GSP/ENTOBRESULTS">
      <xsl:call-template name="onebox"/>
    </xsl:if>
  </xsl:if>

  <!-- *** Handle spelling suggestions, if any *** -->
    <xsl:if test="$show_spelling != '0'">
      <xsl:call-template name="spelling"/>
    </xsl:if>

  <!-- *** Handle synonyms, if any *** -->
    <xsl:if test="$show_synonyms != '0'">
      <xsl:call-template name="synonyms"/>
    </xsl:if>

  <!-- *** Output Google Desktop results (if enabled and any available) *** -->
        <xsl:if test="$egds_show_desktop_results != '0'">
            <xsl:call-template name="desktop_results"/>
        </xsl:if>

  <!-- *** Output results details *** -->
    <div>
    <!-- for keymatch results -->
    <xsl:if test="$show_keymatch != '0'">
      <xsl:apply-templates select="/GSP/GM"/>
    </xsl:if>



<table width="100%"><tr><td>

    <!-- for real results -->
    <xsl:apply-templates select="RES/R">
      <xsl:with-param name="query" select="$query"/>
    </xsl:apply-templates>

 </td><td valign='top' align='right'>
	
		<style type="text/css">
		table.quickFeedbackBorderStyle{
			border-width: 1px 1px 1px 1px;
			border-spacing: 2px;
			border-style: outset outset outset outset;
			border-color: gray gray gray gray;
			border-collapse: separate;
			background-color: white;
		}
		</style>
		
		<link rel="stylesheet" href="http://{FULL_PATH}/feedback.css" type="text/css" media="screen"/>
		
		<script type="text/javascript" src="http://{FULL_PATH}/feedback.js"/>
		
		<div id="quickFeedbackBox">
		<table class='quickFeedbackBorderStyle' align='RIGHT'><tr><td>
			  <span id='quickFeedback'>Please provide feedback on your search experience.
			  <br></br>Please include your email address so we can contact you if necessary.<p></p></span>
			  <textarea tabindex='1' id='quickFeedbackInput'></textarea>
			    <div style='position: relative;'>
			    <input class='send' tabindex='3' onclick='return feedback.quickFeedbackSend()' 
			    type='submit' value='Send' /> <span id='quickFeedback'>
			    This will be sent to <a tabindex='-1' href='mailto:feedback@mycompany.com'>
			    feedback@mycompany.com</a>.</span>
			    </div>
		</td></tr></table>
		</div>

</td></tr></table>


  <!-- *** Filter note (if needed) *** -->
    <xsl:if test="(RES/FI) and (not(RES/NB/NU))">
      <p>
        <i>
        In order to show you the most relevant results,    we have omitted some entries very similar to the <xsl:value-of select="RES/@EN"/> already    displayed.<br/>If you like, you can <a href="{$filter_url}0">    repeat the search with the omitted results included</a>.
        </i>
      </p>
    </xsl:if>
    </div>

  <!-- *** Add bottom navigation *** -->
    <xsl:variable name="nav_style">
      <xsl:choose>
        <xsl:when test="($access='s') or ($access='a')">simple</xsl:when>
        <xsl:otherwise>
          <xsl:value-of select="$choose_bottom_navigation"/>
        </xsl:otherwise>
      </xsl:choose>
    </xsl:variable>

    <xsl:call-template name="google_navigation">
      <xsl:with-param name="prev" select="RES/NB/PU"/>
      <xsl:with-param name="next" select="RES/NB/NU"/>
      <xsl:with-param name="view_begin" select="RES/@SN"/>
      <xsl:with-param name="view_end" select="RES/@EN"/>
      <xsl:with-param name="guess" select="RES/M"/>
      <xsl:with-param name="navigation_style" select="$nav_style"/>
    </xsl:call-template>

  <!-- *** Bottom search box *** -->
    <xsl:if test="$show_bottom_search_box != '0'">
      <xsl:call-template name="bottom_search_box"/>
    </xsl:if>

</xsl:template>
}}}

Make sure to change the 2 {FULL_PATH} placeholders to be the full path of where your css & javascript files reside.

Make sure to change the 'feedback@mycompany.com' email addresses to be the email address where you would like to send your Quick Feedback.

If you run a diff on this XSLT code and the default XSLT code for this section, you will see that it simply adds a table around the search results and puts the Quick Feedback box in the right column of that table aligned to the top.

Now, once your test front end works, you can perform the same search and replace in your current front end stylesheet or custom front end stylesheet. Enjoy!

= Adding only the 'Did you find these results useful?' feedback line to the bottom of your Google Mini or Google Search Appliance Front End =
First, create a new test front end. In this new test front end, search for the section titled: `Output all results` and replace all the text in that section (i.e. up to the section titled `Stopwords suggestions`) with the following:
{{{
<xsl:template name="results">
  <xsl:param name="query"/>
  <xsl:param name="time"/>

  <!-- *** Add top navigation/sort-by bar *** -->
  <xsl:if test="($show_top_navigation != '0') or ($show_sort_by != '0')">
    <xsl:if test="RES"> <!-- there might be onebox results but no RES  -->
      <table width="100%">
      <tr>
      <xsl:if test="$show_top_navigation != '0'">
      <td align="left">
        <xsl:call-template name="google_navigation">
        <xsl:with-param name="prev" select="RES/NB/PU"/>
        <xsl:with-param name="next" select="RES/NB/NU"/>
        <xsl:with-param name="view_begin" select="RES/@SN"/>
        <xsl:with-param name="view_end" select="RES/@EN"/>
        <xsl:with-param name="guess" select="RES/M"/>
        <xsl:with-param name="navigation_style" select="'top'"/>
        </xsl:call-template>
      </td>
      </xsl:if>
      <xsl:if test="$show_sort_by != '0'">
      <td align="right">
      <xsl:call-template name="sort_by"/>
      </td>
      </xsl:if>
      </tr>
    </table>
    </xsl:if>
  </xsl:if>

  <!-- *** Handle OneBox results, if any ***-->
  <xsl:if test="$show_onebox != '0'">
    <xsl:if test="/GSP/ENTOBRESULTS">
      <xsl:call-template name="onebox"/>
    </xsl:if>
  </xsl:if>

  <!-- *** Handle spelling suggestions, if any *** -->
    <xsl:if test="$show_spelling != '0'">
      <xsl:call-template name="spelling"/>
    </xsl:if>

  <!-- *** Handle synonyms, if any *** -->
    <xsl:if test="$show_synonyms != '0'">
      <xsl:call-template name="synonyms"/>
    </xsl:if>

  <!-- *** Output Google Desktop results (if enabled and any available) *** -->
        <xsl:if test="$egds_show_desktop_results != '0'">
            <xsl:call-template name="desktop_results"/>
        </xsl:if>

  <!-- *** Output results details *** -->
    <div>
    <!-- for keymatch results -->
    <xsl:if test="$show_keymatch != '0'">
      <xsl:apply-templates select="/GSP/GM"/>
    </xsl:if>


    <!-- for real results -->
    <xsl:apply-templates select="RES/R">
      <xsl:with-param name="query" select="$query"/>
    </xsl:apply-templates>
	
  <!-- *** Filter note (if needed) *** -->
    <xsl:if test="(RES/FI) and (not(RES/NB/NU))">
      <p>
        <i>
        In order to show you the most relevant results,    we have omitted some entries very similar to the <xsl:value-of select="RES/@EN"/> already    displayed.<br/>If you like, you can <a href="{$filter_url}0">    repeat the search with the omitted results included</a>.
        </i>
      </p>
    </xsl:if>
    </div>

  <!-- *** Add bottom navigation *** -->
    <xsl:variable name="nav_style">
      <xsl:choose>
        <xsl:when test="($access='s') or ($access='a')">simple</xsl:when>
        <xsl:otherwise>
          <xsl:value-of select="$choose_bottom_navigation"/>
        </xsl:otherwise>
      </xsl:choose>
    </xsl:variable>

    <xsl:call-template name="google_navigation">
      <xsl:with-param name="prev" select="RES/NB/PU"/>
      <xsl:with-param name="next" select="RES/NB/NU"/>
      <xsl:with-param name="view_begin" select="RES/@SN"/>
      <xsl:with-param name="view_end" select="RES/@EN"/>
      <xsl:with-param name="guess" select="RES/M"/>
      <xsl:with-param name="navigation_style" select="$nav_style"/>
    </xsl:call-template>


  <link rel="stylesheet" href="http://{FULL_PATH}/feedback.css" type="text/css" media="screen"/>
	
  <script type="text/javascript" src="http://{FULL_PATH}/feedback.js"/>

    <p id='usefulResults'>
    <span id='usefulResultsContent'>
    Did you find these results useful?<br></br>
    <a id='usefulResultsYes'
    onclick='return feedback.usefulResultsYes()'
    href=''>Yes</a> <span class='desc'> | </span>  <a id='usefulResultsNo'
    onclick='return feedback.usefulResultsNo()'
    href=''>No</a>
    </span>
    <br></br></p>


  <!-- *** Bottom search box *** -->
    <xsl:if test="$show_bottom_search_box != '0'">
      <xsl:call-template name="bottom_search_box"/>
    </xsl:if>

</xsl:template>
}}}

If you run a diff on this XSLT code and the default XSLT code for this section, you will see that it simply adds a feedback line to the bottom of the search results.

Now, once your test front end works, you can perform the same search and replace in your current front end stylesheet or custom front end stylesheet.

Enjoy!
