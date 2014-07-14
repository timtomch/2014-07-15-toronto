---
layout: lesson
root: ../..
---

## Selecting Data


<div>
<p>For this lesson, we're going to use the library of the <em>SQL Learners Academy</em> as an example. Since the Academy is much more concerned about teaching SQL than proper cataloguing, its library catalogue is quite rudimentary. The catalogue is based on a few tables that are in relation to each other, enough to get the concept of relational databases across, and run some simple quries!</p>
<p>More importantly, the raw database is always <a href="swclib.db">available for download</a> (in SQLite format) so that students can learn to tinker SQL.</p>
</div>


<div class="objectives">
<h4 id="objectives">Objectives</h4>
<ul>
<li>Explain the difference between a table, a record, and a field.</li>
<li>Explain the difference between a database and a database manager.</li>
<li>Write a query to select all values for specific fields from a single table.</li>
</ul>
</div>

### A Few Definitions


<div>
<p>A <a href="../../gloss.html#relational-database">relational database</a> is a way to store and manipulate information that is arranged as <a href="../../gloss.html#table-database">tables</a>. Each table has columns (also known as <a href="../../gloss.html#field-database">fields</a>) which describe the data, and rows (also known as <a href="../../gloss.html#record-database">records</a>) which contain the data.</p>
<p>When we are using a spreadsheet, we put formulas into cells to calculate new values based on old ones. When we are using a database, we send commands (usually called <a href="../../gloss.html#query">queries</a>) to a <a href="../../gloss.html#database-manager">database manager</a>: a program that manipulates the database for us. The database manager does whatever lookups and calculations the query specifies, returning the results in a tabular form that we can then use as a starting point for further queries.</p>
<blockquote>
<p>Every database manager—Oracle, IBM DB2, PostgreSQL, MySQL, Microsoft Access, and SQLite—stores data in a different way, so a database created with one cannot be used directly by another. However, every database manager can import and export data in a variety of formats, so it <em>is</em> possible to move information from one to another.</p>
</blockquote>
<p>Queries are written in a language called <a href="../../gloss.html#sql">SQL</a>, which stands for &quot;Structured Query Language&quot;. SQL provides hundreds of different ways to analyze and recombine data; we will only look at a handful, but that handful accounts for most of what scientists do.</p>
<p>The tables below show the database we will use in our library example. Students using SQLite can <a href="swclib.db">download</a> the database to try the queries along. Only the first 5 rows of each table are displayed here.</p>
</div>

<div>
<p><strong>Works</strong>: simplistic bibliographical information for the works available in the library</p>
<p>
<table>
  <tr><th>Work_ID</th> <th>Title</th> <th>ISBN</th> <th>Date</th> <th>Place</th> <th>Publisher</th> <th>Edition</th> <th>Pages</th></tr>
<TR><TD>1</TD>
<TD>SQL in a nutshell</TD>
<TD>9780596518844</TD>
<TD>2009</TD>
<TD>Sebastopol</TD>
<TD>O&#39;Reilly</TD>
<TD>3rd ed.</TD>
<TD>578</TD>
</TR>
<TR><TD>2</TD>
<TD>SQL for dummies</TD>
<TD>9781118607961</TD>
<TD>2013</TD>
<TD>Hoboken</TD>
<TD>Wiley</TD>
<TD>8th ed.</TD>
<TD bgcolor="red"></TD>
</TR>
<TR><TD>3</TD>
<TD>PHP &amp; MySQL</TD>
<TD>9781449325572</TD>
<TD>2013</TD>
<TD>Sebastopol</TD>
<TD>O&#39;Reilly</TD>
<TD>2nd ed.</TD>
<TD>532</TD>
</TR>
<TR><TD>4</TD>
<TD>Using SQLite</TD>
<TD>9780596521189</TD>
<TD>2010</TD>
<TD>Sebastopol</TD>
<TD>O&#39;Reilly</TD>
<TD>1st ed.</TD>
<TD>503</TD>
</TR>
<TR><TD>5</TD>
<TD>Geek sublime</TD>
<TD>9780571310302</TD>
<TD>2014</TD>
<TD>London</TD>
<TD>Faber &amp; Faber</TD>
<TD bgcolor="red"></TD>
<TD>258</TD>
</TR>
</table>
</p>
<p><strong>Authors</strong>: the "authority file", containing information about the Authors of the Works</p>
<p>
<table>
  <tr><th>Author_ID</th> <th>Name</th> <th>Surname</th> <th>Occupation</th> <th>Birth</th> <th>Death</th></tr>
<TR><TD>1</TD>
<TD>Kline</TD>
<TD>Kevin E.</TD>
<TD bgcolor="red"></TD>
<TD>1966</TD>
<TD bgcolor="red"></TD>
</TR>
<TR><TD>2</TD>
<TD>Kline</TD>
<TD>Daniel</TD>
<TD bgcolor="red"></TD>
<TD bgcolor="red"></TD>
<TD bgcolor="red"></TD>
</TR>
<TR><TD>3</TD>
<TD>Hunt</TD>
<TD>Brand</TD>
<TD bgcolor="red"></TD>
<TD bgcolor="red"></TD>
<TD bgcolor="red"></TD>
</TR>
<TR><TD>4</TD>
<TD>Taylor</TD>
<TD>Allen G.</TD>
<TD bgcolor="red"></TD>
<TD>1945</TD>
<TD bgcolor="red"></TD>
</TR>
<TR><TD>5</TD>
<TD>McLaughlin</TD>
<TD>Brett</TD>
<TD bgcolor="red"></TD>
<TD bgcolor="red"></TD>
<TD bgcolor="red"></TD>
</TR>
</table>
</p>

<p><strong>Works_Authors</strong>: The relationship between the Works and the Authors (more on that below)</p>
<p>
<table>
  <tr><th>Work_ID</th> <th>Author_ID</th> <th>Role</th></tr>
<TR><TD>1</TD>
<TD>1</TD>
<TD>Author</TD>
</TR>
<TR><TD>1</TD>
<TD>2</TD>
<TD>Contributor</TD>
</TR>
<TR><TD>1</TD>
<TD>3</TD>
<TD>Contributor</TD>
</TR>
<TR><TD>2</TD>
<TD>4</TD>
<TD>Author</TD>
</TR>
<TR><TD>3</TD>
<TD>5</TD>
<TD>Author</TD>
</TR>
</table>
</p>


<p><strong>Items</strong>: the actual copies of the Works owned by the library</p>
<p>
<table>
  <tr><th>Item_ID</th> <th>Work_ID</th> <th>Barcode</th> <th>Acquired</th> <th>Status</th></tr>
<TR><TD>1</TD>
<TD>1</TD>
<TD>081722942611</TD>
<TD>2009</TD>
<TD>Loaned</TD>
</TR>
<TR><TD>2</TD>
<TD>1</TD>
<TD>492437609065</TD>
<TD>2011</TD>
<TD>On shelf</TD>
</TR>
<TR><TD>3</TD>
<TD>2</TD>
<TD>172480710952</TD>
<TD>2013</TD>
<TD>On shelf</TD>
</TR>
<TR><TD>4</TD>
<TD>3</TD>
<TD>708014968732</TD>
<TD>2013</TD>
<TD>Missing</TD>
</TR>
<TR><TD>5</TD>
<TD>3</TD>
<TD>819783404942</TD>
<TD>2014</TD>
<TD>Loaned</TD>
</TR>
</table>
</p>
</div>

<div>
<p>Notice that three entries—one in the <code>Visited</code> table, and two in the <code>Survey</code> table—are shown in red because they don't contain any actual data: we'll return to these missing values <a href="#s:null">later</a>. For now, let's write an SQL query that displays scientists' names. We do this using the SQL command <code>select</code>, giving it the names of the columns we want and the table we want them from. Our query and its output look like this:</p>
</div>


<div class="in">
<pre>%load_ext sqlitemagic</pre>
</div>


<div class="in">
<pre>%%sqlite survey.db
select family, personal from Person;</pre>
</div>

<div class="out">
<pre><table>
<tr><td>Dyer</td><td>William</td></tr>
<tr><td>Pabodie</td><td>Frank</td></tr>
<tr><td>Lake</td><td>Anderson</td></tr>
<tr><td>Roerich</td><td>Valentina</td></tr>
<tr><td>Danforth</td><td>Frank</td></tr>
</table></pre>
</div>


<div>
<p>The semi-colon at the end of the query tells the database manager that the query is complete and ready to run. We have written our commands and column names in lower case, and the table name in Title Case, but we don't have to: as the example below shows, SQL is <a href="../../gloss.html#case-insensitive">case insensitive</a>.</p>
</div>


<div class="in">
<pre>%%sqlite survey.db
SeLeCt FaMiLy, PeRsOnAl FrOm PeRsOn;</pre>
</div>

<div class="out">
<pre><table>
<tr><td>Dyer</td><td>William</td></tr>
<tr><td>Pabodie</td><td>Frank</td></tr>
<tr><td>Lake</td><td>Anderson</td></tr>
<tr><td>Roerich</td><td>Valentina</td></tr>
<tr><td>Danforth</td><td>Frank</td></tr>
</table></pre>
</div>


<div>
<p>Whatever casing convention you choose, please be consistent: complex queries are hard enough to read without the extra cognitive load of random capitalization.</p>
</div>


<div>
<p>Going back to our query, it's important to understand that the rows and columns in a database table aren't actually stored in any particular order. They will always be <em>displayed</em> in some order, but we can control that in various ways. For example, we could swap the columns in the output by writing our query as:</p>
</div>


<div class="in">
<pre>%%sqlite survey.db
select personal, family from Person;</pre>
</div>

<div class="out">
<pre><table>
<tr><td>William</td><td>Dyer</td></tr>
<tr><td>Frank</td><td>Pabodie</td></tr>
<tr><td>Anderson</td><td>Lake</td></tr>
<tr><td>Valentina</td><td>Roerich</td></tr>
<tr><td>Frank</td><td>Danforth</td></tr>
</table></pre>
</div>


<div>
<p>or even repeat columns:</p>
</div>


<div class="in">
<pre>%%sqlite survey.db
select ident, ident, ident from Person;</pre>
</div>

<div class="out">
<pre><table>
<tr><td>dyer</td><td>dyer</td><td>dyer</td></tr>
<tr><td>pb</td><td>pb</td><td>pb</td></tr>
<tr><td>lake</td><td>lake</td><td>lake</td></tr>
<tr><td>roe</td><td>roe</td><td>roe</td></tr>
<tr><td>danforth</td><td>danforth</td><td>danforth</td></tr>
</table></pre>
</div>


<div>
<p>As a shortcut, we can select all of the columns in a table using <code>*</code>:</p>
</div>


<div class="in">
<pre>%%sqlite survey.db
select * from Person;</pre>
</div>

<div class="out">
<pre><table>
<tr><td>dyer</td><td>William</td><td>Dyer</td></tr>
<tr><td>pb</td><td>Frank</td><td>Pabodie</td></tr>
<tr><td>lake</td><td>Anderson</td><td>Lake</td></tr>
<tr><td>roe</td><td>Valentina</td><td>Roerich</td></tr>
<tr><td>danforth</td><td>Frank</td><td>Danforth</td></tr>
</table></pre>
</div>


<div>
<h4 id="challenges">Challenges</h4>
<ol style="list-style-type: decimal">
<li><p>Write a query that selects only site names from the <code>Site</code> table.</p></li>
<li><p>Many people format queries as:</p>
<pre><code>SELECT personal, family FROM person;</code></pre>
<p>or as:</p>
<pre><code>select Personal, Family from PERSON;</code></pre>
<p>What style do you find easiest to read, and why?</p></li>
</ol>
</div>


<div class="keypoints">
<h4 id="key-points">Key Points</h4>
<ul>
<li>A relational database stores information in tables, each of which has a fixed set of columns and a variable number of records.</li>
<li>A database manager is a program that manipulates information stored in a database.</li>
<li>We write queries in a specialized language called SQL to extract information from databases.</li>
<li>SQL is case-insensitive.</li>
</ul>
</div>
