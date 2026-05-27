# Creating OpenSearch Descriptions

Zotero uses OpenSearch 1.1 and OpenURL with some extensions to allow easy specification of locate engines. For a simple OpenURL endpoint, one can use something like:

    <OpenSearchDescription xmlns="http://a9.com/-/spec/opensearch/1.1/">
    <ShortName>CrossRef</ShortName>
    <Description>CrossRef Search Engine</Description>
    <Image width="16" height="16" type="image/x-icon">http://crossref.org/favicon.ico</Image>
    <Url type="text/html" method="GET"
        xmlns:z="http://www.zotero.org/namespaces/openSearch#" 
        template="http://crossref.org/openurl?{z:openURL}"/>
    </OpenSearchDescription>

For a more complicated service:

    <OpenSearchDescription xmlns="http://a9.com/-/spec/opensearch/1.1/">
    <ShortName>Google Scholar</ShortName>
    <Description>Google Scholar Search Engine</Description>
    <Image width="16" height="16" type="image/x-icon">http://scholar.google.com/favicon.ico</Image>
    <Url type="text/html" method="GET"
        xmlns:rft="info:ofi/fmt:kev:mtx:journal" 
        xmlns:z="http://www.zotero.org/namespaces/openSearch#" 
        template="http://scholar.google.com/scholar?as_q=&amp;as_epq={z:title}&amp;as_occt=title&amp;as_sauthors={rft:aufirst?}+{rft:aulast?}&amp;as_ylo={z:year?}&amp;as_yhi={z:year?}&amp;as_sdt=1.&amp;as_sdtp=on&amp;as_sdtf=&amp;as_sdts=22&amp;"/>
    </OpenSearchDescription>

Zotero supports most of the fields specified by the OpenURL specification in any of the following namespaces:

-   `<info:ofi/fmt:kev:mtx:journal>`
-   `<info:ofi/fmt:kev:mtx:book>`
-   `<info:ofi/fmt:kev:mtx:patent>`
-   `<info:ofi/fmt:kev:mtx:sch_svc>`
-   `<info:ofi/fmt:kev:mtx:dissertation>`

These namespaces are treated interchangeably. For a full list of fields, see the [OpenURL documentation](http://www.niso.org/kst/reports/standards/kfile_download?id%3Austring%3Aiso-8859-1=Z39-88-2004.pdf&pt=RkGKiXzW643YeUaYUqZ1BFwDhIG4-24RJbcZBWg8uE4vWdpZsJDs4RjLz0t90_d5_ymGsj_IKVaGZww13HuDlZQ8NBt1sTxP_v4iiGqH7rSaAeVDnMfeKJrrJ_JSEGPB).

Zotero also supports any Zotero field in the Zotero namespace (http://www.zotero.org/namespaces/openSearch#). The following special fields are also supported:

-   openURL - the full OpenURL 1.0 context object
-   year - the year extracted from the Zotero date field

Zotero fields should be used sparingly, as they may not be compatible with other implementations.

For more general details on the OpenSearch description format, see the [OpenSearch documentation](http://www.opensearch.org/Specifications/OpenSearch/1.1).

# Advertising Engines

You can advertise locate engines by adding

    <link rel="search" type="application/x-openurl-opensearchdescription+xml" title="Engine Name" href="http://www.example.com/myengine.xml" />

to the <head> section of any webpage (with the title and href attribute contents modified). The user can then install the locate engine under the locate menu when visiting the webpage.

# Example

An example OpenSearch description for Amazon.com is available [here](http://www.zotero.org/opensearchdemo/opensearch_amazon.html).
