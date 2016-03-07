<h2 id="zotero">Zotero</h2>
<p>Zotero is an amazing tool to save and share resources. It also has helped me from 2010 to gather and export my resources to a bibliography. Like all tools, though, using an API (application programming interface) allows you to tap a tool’s potential. An API is essentially a set of rules, conditions, etc. for a tool that can work anywhere if you are using that API properly.</p>
<p>The great thing about the Zotero API is that it is free and open source, but it also runs on python. This means it has a platform that we merely need to plug and play. So we can create new libraries, add items, etc. But, you may ask, can’t we do this with the user interface as well?</p>
<p>This is true, and like everything in digital humanities it comes down to personal preference. The Zotero API allows you to have more control over data. This may, however, not be something you need so you can stick to the firefox extension or desktop app. But think about the ways the API can be applied. Someone developed a chrome extension that uses Zotero’s API so that chrome users can save resources to their accounts. It is a lightweight extension with no interface like the Firefox one. But you will only ever see the power in something where you can use it.</p>
<h2 id="sparql">SPARQL</h2>
<p>Linked Open Data (LOD) is only as good as its creator. SPARQL, one can argue, creates its only problems: it only works on websites that design LOD, thus needing to be aware of users wanting access to data; it then creates a bit of a feedback loop because only those people with the technical skill and foresight to create LOD actually do it. But a growing number of museums offer their databases through LOD.</p>
<p>SPARQL is a language that queries these databases for you. It uses a structure of <code>subject - object - predicate</code> or in the tutorial example <code>&lt;The Nightwatch&gt; &lt;was created by&gt; &lt;Rembrandt van Rijn&gt;</code>. So you can end up searching for data of a specific kind and then exporting that data to a comma separated file, allowing you to work with it in a variety of platforms. The tutorial suggests you can visualize that data with Palladio.</p>
<p>Several years ago I worked with data on the location of British Roman coins in Gephi. I thought I could query the museum for coins between 400 and 800 and I limited the count to 100 because it is mega slow.</p>
<pre><code># Return object links and creation date
PREFIX bmo: &lt;http://collection.britishmuseum.org/id/ontology/&gt;
PREFIX skos: &lt;http://www.w3.org/2004/02/skos/core#&gt;
PREFIX ecrm: &lt;http://erlangen-crm.org/current/&gt;
PREFIX xsd: &lt;http://www.w3.org/2001/XMLSchema#&gt;
SELECT ?object ?date
WHERE {

  # We'll use our previous command to search only for objects of type "print"
  ?object bmo:PX_object_type ?object_type .
  ?object_type skos:prefLabel "coin" .

  # We need to link though several nodes to find the creation date associated
  # with an object
  ?object ecrm:P108i_was_produced_by ?production .
  ?production ecrm:P9_consists_of ?date_node .
  ?date_node ecrm:P4_has_time-span ?timespan .
  ?timespan ecrm:P82a_begin_of_the_begin ?date .

  # Yes, we need to connect quite a few dots to get to the date node! Now that
  # we have it, we can filter our results. Because we are filtering a date, we
  # must attach the xsd:date tag to our date strings so that SPARQL knows how to
  # parse them.

  FILTER(?date &gt;= "400-01-01"^^xsd:date &amp;&amp; ?date &lt;= "800-01-01"^^xsd:date)
}

LIMIT 100
</code></pre>
<p>Unfortunately I got nothing after waiting a very long time.</p>
<p>So I changed the date to <code>FILTER(?date &gt;= "1280-01-01"^^xsd:date &amp;&amp; ?date &lt;= "1300-01-01"^^xsd:date)</code> and everything worked!</p>
<p>I tried adding my work to Palladio but I just received garbled nonsense about HTML</p>
