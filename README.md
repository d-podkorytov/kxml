kXML
====

A stable XML parsing/generating library for D. It attemps to conform to XML 1.0 specification.

It makes a GC allocation for each node, which can trigger many GC collections, so it is not recommended for performance-sensitive applications (parsing 1000+ XML documents per second).

TODO:
-----

- support full paths for both sides of the inequality (start with one side...)
- support * for nodes and attributes
- support \[#] (note that this is a 1-based index)
- support [@*] to catch all nodes with attributes
- support [not(@*)] to catch all nodes with no attributes
- support [last()] with math (yes, it's lame)? (last is $-1)
- add <?xml ?> XmlPI to the beginning of xmldocuments


Usage examples:
---------------
```D
/// Demonstrates the use of XPath to find XML nodes
string xmlstring = `<table class="table1">
					  <tr><th>URL</th><td><a href="path1/path2">Link 1.1</a></td></tr>
					  <tr ab="two"><th>Head</th><td>Text 1.2</td></tr>
					  <tr ab="4"><th>Head</th><td>Text 1.3</td></tr>
					</table>
					<table class="table2">
					  <tr><th>URL </th><td><a href="path1/path2">Link 2.1</a></td></tr>
					  <tr ab="six"><th>Head</th><td>Text 2.2</td></tr>
					  <tr ab="9"><th>Head</th><td>Text 2.3</td></tr>
					</table>`;
XmlNode xml = readDocument(xmlstring);
XmlNode[] searchlist = xml.parseXPath(`//tr[@ab<=7]/td`);
assert(searchlist.length == 1 && searchlist[0].getCData == "Text 1.3");
assert(xml.toString() == xmlstring);

/// Demonstrates a search with multiple node results
string xmlstring = `<message responseID="1234abcd" text="weather 12345" type="message" order="5">
						<flags>triggered</flags>
						<flags>targeted</flags>
					</message>`;
XmlNode xml = xmlstring.readDocument();
XmlNode[] searchlist = xml.parseXPath("message/flags");
assert(searchlist.length == 2 && searchlist[0].getName == "flags");
```
