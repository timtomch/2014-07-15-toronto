---
layout: lesson
root: ../..
---

## Filtering


<div class="objectives">
<h4 id="objectives">Objectives</h4>
<ul>
<li>Write queries that select records that satisfy user-specified conditions.</li>
<li>Explain the order in which the clauses in a query are executed.</li>
</ul>
</div>


<div>
<p>One of the most powerful features of a database is the ability to <a href="../../gloss.html#filter">filter</a> data, i.e., to select only those records that match certain criteria. For example, suppose we want to list all the books in the catalogue that have been published by Wiley. We can select these records from the <code>Works</code> table by using a <code>WHERE</code> clause in our query:</p>
</div>


<div class="in">
<pre>%load_ext sqlitemagic</pre>
</div>


<div class="in">
<pre>%%sqlite swclib.db
SELECT * FROM Works WHERE Publisher="Wiley";</pre>
</div>

<div class="out">
<pre><table>
	<TR><TD>2</TD>
	<TD>SQL for dummies</TD>
	<TD>9781118607961</TD>
	<TD>2013</TD>
	<TD>Hoboken</TD>
	<TD>Wiley</TD>
	<TD>8th ed.</TD>
	<TD></TD>
	</TR>
	<TR><TD>8</TD>
	<TD>Discovering SQL</TD>
	<TD>9781118002674</TD>
	<TD>2011</TD>
	<TD>Indianapolis</TD>
	<TD>Wiley</TD>
	<TD></TD>
	<TD>400</TD>
	</TR>
	<TR><TD>11</TD>
	<TD>SQL bible</TD>
	<TD>9780470229064</TD>
	<TD>2008</TD>
	<TD>Indianapolis</TD>
	<TD>Wiley</TD>
	<TD>2nd ed.</TD>
	<TD>857</TD>
	</TR>
	<TR><TD>13</TD>
	<TD>SQL for dummies</TD>
	<TD>9780470557419</TD>
	<TD>2010</TD>
	<TD>Hoboken</TD>
	<TD>Wiley</TD>
	<TD>7th ed.</TD>
	<TD>440</TD>
	</TR>
	<TR><TD>15</TD>
	<TD>Beginning SQL</TD>
	<TD>0764577328</TD>
	<TD>2005</TD>
	<TD>Indianapolis</TD>
	<TD>Wiley</TD>
	<TD></TD>
	<TD>501</TD>
	</TR>
	<TR><TD>17</TD>
	<TD>SQL all-in-one</TD>
	<TD>9780470929964</TD>
	<TD>2011</TD>
	<TD>Hoboken</TD>
	<TD>Wiley</TD>
	<TD>2nd ed.</TD>
	<TD>708</TD>
	</TR>
	<TR><TD>18</TD>
	<TD>Access 2013 all-in-one</TD>
	<TD>9781118510551</TD>
	<TD>2013</TD>
	<TD>Hoboken</TD>
	<TD>Wiley</TD>
	<TD></TD>
	<TD>760</TD>
	</TR>
</table></pre>
</div>


<div>
<p>The database manager executes this query in two stages. First, it checks at each row in the <code>Works</code> table to see which ones satisfy the <code>WHERE</code>. It then uses the column names following the <code>SELECT</code> keyword to determine what columns to display.</p>
</div>


<div>
<p>This processing order means that we can filter records using <code>WHERE</code> based on values in columns that aren't then displayed:</p>
</div>


<div class="in">
<pre>%%sqlite survey.db
SELECT Title FROM Works WHERE Publisher="Wiley";</pre>
</div>

<div class="out">
<pre><table>
	<TR><TD>SQL for dummies</TD>
	</TR>
	<TR><TD>Discovering SQL</TD>
	</TR>
	<TR><TD>SQL bible</TD>
	</TR>
	<TR><TD>SQL for dummies</TD>
	</TR>
	<TR><TD>Beginning SQL</TD>
	</TR>
	<TR><TD>SQL all-in-one</TD>
	</TR>
	<TR><TD>Access 2013 all-in-one</TD>
	</TR>
</table></pre>
</div>


<!--><div>
<p><img src="img/sql-filter.svg" alt="SQL Filtering in Action" /></p>
</div>-->


<div>
<p>We can use many other Boolean operators to filter our data. For example, we can display only titles that have been published by Wiley in 2012 or later:</p>
</div>


<div class="in">
<pre>%%sqlite swclib.db
SELECT * FROM Works WHERE (Publisher='Wiley') AND (Date>='2011');</pre>
</div>

<div class="out">
<pre><table>
	<TR><TD>2</TD>
	<TD>SQL for dummies</TD>
	<TD>9781118607961</TD>
	<TD>2013</TD>
	<TD>Hoboken</TD>
	<TD>Wiley</TD>
	<TD>8th ed.</TD>
	<TD></TD>
	</TR>
	<TR><TD>8</TD>
	<TD>Discovering SQL</TD>
	<TD>9781118002674</TD>
	<TD>2011</TD>
	<TD>Indianapolis</TD>
	<TD>Wiley</TD>
	<TD></TD>
	<TD>400</TD>
	</TR>
	<TR><TD>17</TD>
	<TD>SQL all-in-one</TD>
	<TD>9780470929964</TD>
	<TD>2011</TD>
	<TD>Hoboken</TD>
	<TD>Wiley</TD>
	<TD>2nd ed.</TD>
	<TD>708</TD>
	</TR>
	<TR><TD>18</TD>
	<TD>Access 2013 all-in-one</TD>
	<TD>9781118510551</TD>
	<TD>2013</TD>
	<TD>Hoboken</TD>
	<TD>Wiley</TD>
	<TD></TD>
	<TD>760</TD>
	</TR>
</table></pre>
</div>


<div>
<p>(The parentheses around the individual tests aren't strictly required, but they help make the query easier to read.)</p>
<blockquote>
<p>Most database managers have a special data type for dates. In fact, many have two: one for dates, such as &quot;May 31, 1971&quot;, and one for durations, such as &quot;31 days&quot;. SQLite doesn't: instead, it stores dates as either text (in the ISO-8601 standard format &quot;YYYY-MM-DD HH:MM:SS.SSSS&quot;), real numbers (the number of days since November 24, 4714 BCE), or integers (the number of seconds since midnight, January 1, 1970). If this sounds complicated, it is, but not nearly as complicated as figuring out <a href="http://en.wikipedia.org/wiki/Swedish_calendar">historical dates in Sweden</a>. In our library example, we make this even easier by storing only years (as integers) instead of full dates.</p>
</blockquote>
</div>


<div>
<p>If we want to list all books published either by Wiley or O'Reilly, we can combine the query using <code>OR</code>. Note in passing how the apostrophe in the name O'Reilly is escaped:</p>
</div>


<div class="in">
<pre>%%sqlite swclib.db
SELECT * FROM Works WHERE Publisher='Wiley' OR Publisher='O''Reilly';</pre>
</div>

<div class="out">
<pre><table>
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
	<TD></TD>
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
	<TR><TD>8</TD>
	<TD>Discovering SQL</TD>
	<TD>9781118002674</TD>
	<TD>2011</TD>
	<TD>Indianapolis</TD>
	<TD>Wiley</TD>
	<TD></TD>
	<TD>400</TD>
	</TR>
	<TR><TD>11</TD>
	<TD>SQL bible</TD>
	<TD>9780470229064</TD>
	<TD>2008</TD>
	<TD>Indianapolis</TD>
	<TD>Wiley</TD>
	<TD>2nd ed.</TD>
	<TD>857</TD>
	</TR>
	<TR><TD>12</TD>
	<TD>Learning SQL</TD>
	<TD>9780596520830</TD>
	<TD>2009</TD>
	<TD>Sebastopol</TD>
	<TD>O&#39;Reilly</TD>
	<TD>2nd ed.</TD>
	<TD>320</TD>
	</TR>
	<TR><TD>13</TD>
	<TD>SQL for dummies</TD>
	<TD>9780470557419</TD>
	<TD>2010</TD>
	<TD>Hoboken</TD>
	<TD>Wiley</TD>
	<TD>7th ed.</TD>
	<TD>440</TD>
	</TR>
	<TR><TD>15</TD>
	<TD>Beginning SQL</TD>
	<TD>0764577328</TD>
	<TD>2005</TD>
	<TD>Indianapolis</TD>
	<TD>Wiley</TD>
	<TD></TD>
	<TD>501</TD>
	</TR>
	<TR><TD>17</TD>
	<TD>SQL all-in-one</TD>
	<TD>9780470929964</TD>
	<TD>2011</TD>
	<TD>Hoboken</TD>
	<TD>Wiley</TD>
	<TD>2nd ed.</TD>
	<TD>708</TD>
	</TR>
	<TR><TD>18</TD>
	<TD>Access 2013 all-in-one</TD>
	<TD>9781118510551</TD>
	<TD>2013</TD>
	<TD>Hoboken</TD>
	<TD>Wiley</TD>
	<TD></TD>
	<TD>760</TD>
	</TR>
	<TR><TD>19</TD>
	<TD>SQL in a nutshell</TD>
	<TD>0596004818</TD>
	<TD>2004</TD>
	<TD>Cambridge</TD>
	<TD>O&#39;Reilly</TD>
	<TD>2nd ed.</TD>
	<TD>691</TD>
	</TR>
	<TR><TD>20</TD>
	<TD>MySQL in a nutshell</TD>
	<TD>0596007892</TD>
	<TD>2005</TD>
	<TD>Sebastopol</TD>
	<TD>O&#39;Reilly</TD>
	<TD>1st ed.</TD>
	<TD>321</TD>
	</TR>

</table></pre>
</div>

<div>
<p>Alternatively, we can use <code>IN</code> to see if a value is in a specific set:</p>
</div>


<div class="in">
<pre>%%sqlite swclib.db
SELECT * FROM Works WHERE Publisher IN('Wiley','O''Reilly');</pre>
</div>

<div class="out">
<pre><table>
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
	<TD></TD>
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
	<TR><TD>8</TD>
	<TD>Discovering SQL</TD>
	<TD>9781118002674</TD>
	<TD>2011</TD>
	<TD>Indianapolis</TD>
	<TD>Wiley</TD>
	<TD></TD>
	<TD>400</TD>
	</TR>
	<TR><TD>11</TD>
	<TD>SQL bible</TD>
	<TD>9780470229064</TD>
	<TD>2008</TD>
	<TD>Indianapolis</TD>
	<TD>Wiley</TD>
	<TD>2nd ed.</TD>
	<TD>857</TD>
	</TR>
	<TR><TD>12</TD>
	<TD>Learning SQL</TD>
	<TD>9780596520830</TD>
	<TD>2009</TD>
	<TD>Sebastopol</TD>
	<TD>O&#39;Reilly</TD>
	<TD>2nd ed.</TD>
	<TD>320</TD>
	</TR>
	<TR><TD>13</TD>
	<TD>SQL for dummies</TD>
	<TD>9780470557419</TD>
	<TD>2010</TD>
	<TD>Hoboken</TD>
	<TD>Wiley</TD>
	<TD>7th ed.</TD>
	<TD>440</TD>
	</TR>
	<TR><TD>15</TD>
	<TD>Beginning SQL</TD>
	<TD>0764577328</TD>
	<TD>2005</TD>
	<TD>Indianapolis</TD>
	<TD>Wiley</TD>
	<TD></TD>
	<TD>501</TD>
	</TR>
	<TR><TD>17</TD>
	<TD>SQL all-in-one</TD>
	<TD>9780470929964</TD>
	<TD>2011</TD>
	<TD>Hoboken</TD>
	<TD>Wiley</TD>
	<TD>2nd ed.</TD>
	<TD>708</TD>
	</TR>
	<TR><TD>18</TD>
	<TD>Access 2013 all-in-one</TD>
	<TD>9781118510551</TD>
	<TD>2013</TD>
	<TD>Hoboken</TD>
	<TD>Wiley</TD>
	<TD></TD>
	<TD>760</TD>
	</TR>
	<TR><TD>19</TD>
	<TD>SQL in a nutshell</TD>
	<TD>0596004818</TD>
	<TD>2004</TD>
	<TD>Cambridge</TD>
	<TD>O&#39;Reilly</TD>
	<TD>2nd ed.</TD>
	<TD>691</TD>
	</TR>
	<TR><TD>20</TD>
	<TD>MySQL in a nutshell</TD>
	<TD>0596007892</TD>
	<TD>2005</TD>
	<TD>Sebastopol</TD>
	<TD>O&#39;Reilly</TD>
	<TD>1st ed.</TD>
	<TD>321</TD>
	</TR>
</table></pre>
</div>


<div>
<p>We can combine <code>AND</code> with <code>OR</code>, but we need to be careful about which operator is executed first. If we <em>don't</em> use parentheses, we get this:</p>
</div>


<div class="in">
<pre>%%sqlite swclib.db
SELECT * FROM Works WHERE Publisher='Wiley' OR Publisher='O''Reilly' AND Date>=2012;</pre>
</div>

<div class="out">
<pre><table>
	<TR><TD>2</TD>
	<TD>SQL for dummies</TD>
	<TD>9781118607961</TD>
	<TD>2013</TD>
	<TD>Hoboken</TD>
	<TD>Wiley</TD>
	<TD>8th ed.</TD>
	<TD></TD>
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
	<TR><TD>8</TD>
	<TD>Discovering SQL</TD>
	<TD>9781118002674</TD>
	<TD>2011</TD>
	<TD>Indianapolis</TD>
	<TD>Wiley</TD>
	<TD></TD>
	<TD>400</TD>
	</TR>
	<TR><TD>11</TD>
	<TD>SQL bible</TD>
	<TD>9780470229064</TD>
	<TD>2008</TD>
	<TD>Indianapolis</TD>
	<TD>Wiley</TD>
	<TD>2nd ed.</TD>
	<TD>857</TD>
	</TR>
	<TR><TD>13</TD>
	<TD>SQL for dummies</TD>
	<TD>9780470557419</TD>
	<TD>2010</TD>
	<TD>Hoboken</TD>
	<TD>Wiley</TD>
	<TD>7th ed.</TD>
	<TD>440</TD>
	</TR>
	<TR><TD>15</TD>
	<TD>Beginning SQL</TD>
	<TD>0764577328</TD>
	<TD>2005</TD>
	<TD>Indianapolis</TD>
	<TD>Wiley</TD>
	<TD></TD>
	<TD>501</TD>
	</TR>
	<TR><TD>17</TD>
	<TD>SQL all-in-one</TD>
	<TD>9780470929964</TD>
	<TD>2011</TD>
	<TD>Hoboken</TD>
	<TD>Wiley</TD>
	<TD>2nd ed.</TD>
	<TD>708</TD>
	</TR>
	<TR><TD>18</TD>
	<TD>Access 2013 all-in-one</TD>
	<TD>9781118510551</TD>
	<TD>2013</TD>
	<TD>Hoboken</TD>
	<TD>Wiley</TD>
	<TD></TD>
	<TD>760</TD>
	</TR>
</table></pre>
</div>


<div>
<p>which is salinity measurements by Lake, and <em>any</em> measurement by Roerich. We probably want this instead:</p>
</div>


<div class="in">
<pre>%%sqlite survey.db
select * from Survey where quant=&#39;sal&#39; and (person=&#39;lake&#39; or person=&#39;roe&#39;);</pre>
</div>

<div class="out">
<pre><table>
<tr><td>734</td><td>lake</td><td>sal</td><td>0.05</td></tr>
<tr><td>751</td><td>lake</td><td>sal</td><td>0.1</td></tr>
<tr><td>752</td><td>lake</td><td>sal</td><td>0.09</td></tr>
<tr><td>752</td><td>roe</td><td>sal</td><td>41.6</td></tr>
<tr><td>837</td><td>lake</td><td>sal</td><td>0.21</td></tr>
<tr><td>837</td><td>roe</td><td>sal</td><td>22.5</td></tr>
</table></pre>
</div>


<div>
<p>Finally, we can use <code>distinct</code> with <code>where</code> to give a second level of filtering:</p>
</div>


<div class="in">
<pre>%%sqlite survey.db
select distinct person, quant from Survey where person=&#39;lake&#39; or person=&#39;roe&#39;;</pre>
</div>

<div class="out">
<pre><table>
<tr><td>lake</td><td>sal</td></tr>
<tr><td>lake</td><td>rad</td></tr>
<tr><td>lake</td><td>temp</td></tr>
<tr><td>roe</td><td>sal</td></tr>
<tr><td>roe</td><td>rad</td></tr>
</table></pre>
</div>


<div>
<p>But remember: <code>distinct</code> is applied to the values displayed in the chosen columns, not to the entire rows as they are being processed.</p>
<blockquote>
<p>What we have just done is how most people &quot;grow&quot; their SQL queries. We started with something simple that did part of what we wanted, then added more clauses one by one, testing their effects as we went. This is a good strategy—in fact, for complex queries it's often the <em>only</em> strategy—but it depends on quick turnaround, and on us recognizing the right answer when we get it.</p>
<p>The best way to achieve quick turnaround is often to put a subset of data in a temporary database and run our queries against that, or to fill a small database with synthesized records. For example, instead of trying our queries against an actual database of 20 million Australians, we could run it against a sample of ten thousand, or write a small program to generate ten thousand random (but plausible) records and use that.</p>
</blockquote>
</div>


<div>
<h4 id="challenges">Challenges</h4>
<ol style="list-style-type: decimal">
<li><p>Suppose we want to select all sites that lie more than 30° from the poles. Our first query is:</p>
<pre><code>select * from Site where (lat &gt; -60) or (lat &lt; 60);</code></pre>
<p>Explain why this is wrong, and rewrite the query so that it is correct.</p></li>
<li><p>Normalized salinity readings are supposed to be between 0.0 and 1.0. Write a query that selects all records from <code>Survey</code> with salinity values outside this range.</p></li>
<li><p>The SQL test <code>*column-name* like *pattern*</code> is true if the value in the named column matches the pattern given; the character '%' can be used any number of times in the pattern to mean &quot;match zero or more characters&quot;.</p>
<table>
  <tr> <th>
Expression
</th> <th>
Value
</th> </tr>
  <tr> <td>
<code>'a' like 'a'</code>
</td> <td>
True
</td> </tr>
  <tr> <td>
<code>'a' like '%a'</code>
</td> <td>
True
</td> </tr>
  <tr> <td>
<code>'b' like '%a'</code>
</td> <td>
False
</td> </tr>
  <tr> <td>
<code>'alpha' like 'a%'</code>
</td> <td>
True
</td> </tr>
  <tr> <td>
<code>'alpha' like 'a%p%'</code>
</td> <td>
True
</td> </tr>
</table>
<p>The expression <code>*column-name* not like *pattern*</code> inverts the test. Using <code>like</code>, write a query that finds all the records in <code>Visited</code> that <em>aren't</em> from sites labelled 'DR-something'.</p></li>
</ol>
</div>


<div class="keypoints">
<h4 id="key-points">Key Points</h4>
<ul>
<li>Use <code>where</code> to filter records according to Boolean conditions.</li>
<li>Filtering is done on whole records, so conditions can use fields that are not actually displayed.</li>
</ul>
</div>
